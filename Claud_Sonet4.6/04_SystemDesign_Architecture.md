# Interview Prep — File 4 of 6
# System Design & Architecture

> **Rule:** Never start with architecture. Always open with: Goals → Metrics → Constraints → NFRs → Levers → Options → Architecture
> **Rule:** Every design decision needs a trade-off. No trade-off = not senior-level thinking.

---

## THE UNIVERSAL SYSTEM DESIGN OPENER

> "Before I jump into the design, I want to spend two to three minutes clarifying goals, success metrics, constraints, and NFRs — because every architecture decision flows from these. Is that okay?"

This signal alone separates senior engineers from junior ones. Junior candidates draw boxes immediately. Senior engineers ask questions first.

---

## DESIGN FRAMEWORK — APPLY TO EVERY QUESTION

```
Step 1: GOAL        — What problem are we solving? Who are the users?
Step 2: METRICS     — How do we know we succeeded? (latency, throughput, accuracy)
Step 3: CONSTRAINTS — What cannot change? (existing systems, budget, compliance)
Step 4: NFRs        — Scalability, reliability, availability, security, idempotency
Step 5: LEVERS      — Break system into 4-5 control points
Step 6: OPTIONS     — A (simple), B (recommended), C (complex) — pick B with reasoning
Step 7: ARCHITECTURE — Draw the components, show the data flow
Step 8: FAILURE SCENARIOS — What breaks? How do we handle it?
Step 9: TRADE-OFFS  — What did we sacrifice? What alternatives exist?
Step 10: PHASES     — How do we build this incrementally?
```

---

## Q1: Design a Real-Time Inventory Tracking System (TJX WMS Context)

### Step 1 — Goal
Track inventory movement in real time across distribution center zones — supporting pick, pack, ship workflows and feeding Manhattan WMS and Oracle ERP.

### Step 2 — Success Metrics
| Metric | Target |
|--------|--------|
| Scan-to-system latency | < 500ms (p99) |
| Inventory accuracy vs physical count | > 99.5% |
| System availability | 99.9% |
| Current stock query response | < 100ms |
| Duplicate scan handling | 100% idempotent |

### Step 3 — Constraints
- Must integrate with Manhattan WMS — cannot replace it
- Barcode scanners are edge devices — DC network can drop
- Peak load is 3–5x normal during inbound shifts
- SOX compliance requires immutable audit trail

### Step 4 — NFRs
- Reliability: zero scan event loss (audit + accuracy requirement)
- Idempotency: scanners retransmit on network drop
- Availability: 99.9% — DC operations depend on this
- Auditability: every stock movement traceable

### Step 5 — Levers (Control Points)
| Lever | Challenge | Target Outcome |
|-------|-----------|---------------|
| Scan Ingestion | Network drops, duplicate scans | Reliable, idempotent event capture |
| Inventory State | Concurrent zone updates | Accurate, conflict-free stock levels |
| Reservation | Pick conflicts, overselling | Consistent reservations |
| Query Layer | WMS needs sub-100ms reads | Redis cache with fallthrough |
| Audit | SOX requirements | Immutable append-only store |

### Step 6 — Options
- **Option A:** Extend Manhattan WMS — fast, but WMS not built for event throughput. Rejected.
- **Option B (Recommended):** Event-driven ingestion layer above WMS, Redis read layer decoupled from WMS writes. Balances integration reality with performance.
- **Option C:** Full event sourcing + CQRS — maximum flexibility, too complex for Phase 1.

### Step 7 — Architecture
```
DC Floor (Scanners / RFID / Handhelds)
    │
    ▼
Edge Gateway (per DC zone)
    Buffers up to 10K events on network drop
    Assigns idempotency key: deviceId + timestamp + barcode
    Lightweight validation: valid SKU, known zone
    │
    │ Kafka: scan-events topic
    ▼
Scan Processing Service
    Checks idempotency key against Redis dedup cache (24hr TTL)
    Enriches with SKU master data
    Classifies: RECEIVE / PICK / PACK / SHIP / ADJUST
    │
    ├──► Inventory State Service (Azure SQL)
    │       Optimistic locking — prevents concurrent pick conflicts
    │       Publishes INVENTORY_UPDATED events
    │
    ├──► Redis Cache (sub-100ms reads)
    │       LRU eviction for high SKU churn
    │       Manhattan WMS picker allocation queries here
    │
    ├──► Audit Event Store (Cosmos DB / S3 — append only)
    │       SOX compliance, shrinkage investigation
    │
    └──► WMS Sync Service
            Keeps Manhattan WMS in sync
            DLQ for failed syncs
            Alert if lag > 5 minutes
```

### Step 8 — Failure Scenarios
| Failure | Impact | Mitigation |
|---------|--------|------------|
| Kafka down | Processing pauses | Edge buffer replays on recovery |
| Redis down | Slow queries | Cache miss falls through to SQL |
| WMS sync lag | WMS shows stale data | Redis is live source; DLQ retries |
| Duplicate scans | Double decrement risk | Idempotency key blocks at ingestion |
| Concurrent picks | Oversell risk | Optimistic locking + retry |

### Step 9 — Key Code Patterns

**Idempotency:**
```java
ScanEvent event = ScanEvent.builder()
    .idempotencyKey(deviceId + ":" + scanTimestamp + ":" + barcode)
    .build();

if (redisDedup.exists(event.getIdempotencyKey())) {
    return; // silent skip — scanner gets success ack
}
redisDedup.set(event.getIdempotencyKey(), "seen", Duration.ofHours(24));
```

**Optimistic Locking:**
```java
@Transactional
void processPick(String skuId, String zoneId, int qty) {
    InventoryRecord record = repo.findBySkuAndZone(skuId, zoneId);
    if (record.getAvailableQty() < qty) {
        throw new InsufficientStockException();
    }
    record.setAvailableQty(record.getAvailableQty() - qty);
    record.setVersion(record.getVersion() + 1); // version bump for optimistic lock
    repo.save(record);
    // Second concurrent save → OptimisticLockException → retry → sees updated count
}
```

**Closing Statement:**
> "The core principle: capture every scan reliably at the edge, process exactly once in the core, serve sub-100ms from cache, sync to WMS asynchronously — DC operations never blocked by system latency."

---

## Q2: SAGA Pattern — WMS Fulfillment Flow

### The Problem
Fulfilling an order touches 4 services, each with its own database. A single @Transactional cannot span all four.

```
Inventory Service  → inventory_reservations (Azure SQL)
Warehouse Service  → pick_tasks             (Azure SQL)
Shipping Service   → shipments              (Cosmos DB)
Dispatch Service   → dispatch_confirmations (Azure SQL)
```

### SAGA Orchestration
```
Order Fulfillment Orchestrator
        │
Step 1: Reserve Inventory  →  SUCCESS: proceed  |  FAIL: end saga
Step 2: Create Pick Task   →  SUCCESS: proceed  |  FAIL: release inventory
Step 3: Generate Shipment  →  SUCCESS: proceed  |  FAIL: cancel pick + release inventory
Step 4: Confirm Dispatch   →  SUCCESS: COMPLETE |  FAIL: compensate all three steps
```

Each compensation is a real business operation, not a DB rollback.

### Outbox Pattern — Why SAGA Alone Is Not Enough

**The gap:** A crash between DB write and Kafka publish creates ghost state.
```java
@Transactional  // ONE atomic transaction — both writes or neither
void reserveInventory(String sku, int qty) {
    inventoryRepo.save(new Reservation(sku, qty, RESERVED));
    outboxRepo.save(OutboxEvent.builder()
        .eventType("INVENTORY_RESERVED")
        .status(PENDING).build());
}
// Separate poller publishes PENDING events to Kafka with retry
```

**When to use which:**
| Step | Transactional Core | Event Via Outbox |
|------|--------------------|-----------------|
| Reserve Inventory | Strong ACID (no double reserve) | INVENTORY_RESERVED |
| Create Pick Task | Task must exist before picker acts | PICK_TASK_CREATED |
| Generate Shipment | ID needed for carrier/label | SHIPMENT_GENERATED |
| Confirm Dispatch | Eventual — scanner is source of truth | Event-driven |

---

## Q3: Monolith vs Microservices

**Memory Hook:** Team + Domain + Scale → Architecture Choice

> "I do not default to microservices. The decision depends on three factors: team structure, domain complexity, and scaling granularity needed.
>
> Monolith is right when the domain is unified, the team is small, change frequency is low, and scale is modest. My Bank of America campaign tool served 1,000 to 2,000 internal users — microservices would have added operational overhead with zero benefit.
>
> Microservices are right when different parts of the system have genuinely different scaling needs and different change rates. At Optum, find-membership had 5 million weekly calls. Claims had a completely different load pattern. A monolith forces you to scale everything together.
>
> On 'can we scale a monolith behind a gateway?' — yes. The problem is granularity. If eligibility is the bottleneck, you scale the entire monolith, paying for compute claims and benefits do not need. Kubernetes gives pod-level precision.
>
> My preference: start modular monolith, extract to microservices when domain boundaries are clear, team structure supports it, and observability is in place. Premature microservices is a common enterprise anti-pattern."

**Trade-offs table:**
| | Monolith | Microservices |
|--|---------|--------------|
| Build speed | Fast initially | Slower — more infrastructure |
| Debugging | Easy | Hard — distributed tracing required |
| Scaling | Coarse-grained | Fine-grained |
| Team independence | Low | High |
| Operational overhead | Low | High |

---

## Q4: Design for Scalability — From 120 to 1200 Customers

**Memory Hook:** Diagnose → Quick Wins → Horizontal → Data → Ops → Team → Incremental

> "Scaling 10x is primarily an organizational and operational challenge, not just a technology challenge. Cloud handles compute elasticity — that is the easy part."

**7-Step Framework:**

**1. Diagnose first — find the actual bottleneck**
> "Before scaling, profile: is it DB, a specific service, or network? Scale the constraint, not everything."

Real example: When Care Management grew from 120 to 400 clients, OpenSearch latency climbed from 80ms to 700ms. Root cause: hot shards — all tenants shared one index. We introduced tenant-based index routing and time-based rollover — latency back to 95ms.

**2. Quick wins before re-architecture**
- Redis caching for read-heavy data
- Connection pool tuning (HikariCP for Oracle)
- Query optimization (execution plans, composite indexes)
- CDN for static assets

**3. Horizontal scaling**
- Stateless services → Kubernetes HPA
- Load balancer tuning
- AKS node pool scaling

**4. Data layer scaling**
- Read replicas for read-heavy workloads
- Tenant-based partitioning in Oracle
- Index-per-tenant or time-based in OpenSearch
- Archive cold data (S3 lifecycle policies)

**5. Operational readiness**
- SLOs defined per service before the next customer cohort
- Alerting on p99 latency, error rate, Kafka consumer lag
- Runbooks for every tier-1 failure mode

**6. Team scaling**
- Domain-based ownership alignment
- Platform team for shared infra
- 30–40% headcount growth for 10x customer growth (not 1:1)

**7. Incremental rollout**
- Scale one layer at a time
- Load test before each major customer onboarding
- Canary deployments for risky changes

---

## Q5: Backpressure Handling

**Memory Hook:** Rate Limit → Circuit Breaker → Queue Buffer → Reactive → Bulkhead

**Simple analogy:** Water tank fills at 100L/min, pipe leads to a bucket emptying at 20L/min. Without flow control the bucket overflows. Backpressure is the signal from bucket to tank: slow down.

**5 Strategies:**

**1. Rate Limiting — reject early at the gate**
```java
RateLimiter.of("api", RateLimiterConfig.custom()
    .limitForPeriod(1000)
    .limitRefreshPeriod(Duration.ofSeconds(1))
    .build());
// Exceeds limit → 429 Too Many Requests — clean, fast failure
```

**2. Circuit Breaker — stop calling a failing service**
```
CLOSED (normal) → OPEN (too many failures, fast fail) → HALF_OPEN (test recovery)
```
```java
CircuitBreaker.of("inventory", CircuitBreakerConfig.custom()
    .failureRateThreshold(50)          // open if 50% fail
    .waitDurationInOpenState(Duration.ofSeconds(30))
    .build());
```

**3. Queue Buffering — absorb the spike**
Kafka between producer and consumer. Consumer pulls at its own pace. Monitor consumer lag in New Relic — alert when lag exceeds threshold.

**4. Reactive Streams — demand-based flow control**
```java
Flux.fromIterable(events)
    .onBackpressureBuffer(1000)
    .flatMap(event -> process(event), 16) // max 16 concurrent
    .subscribe();
```

**5. Bulkhead — isolate failures to a pool**
```java
Bulkhead.of("opensearch", BulkheadConfig.custom()
    .maxConcurrentCalls(20).build());
// OpenSearch slow → 20 threads affected → other services unaffected
```

**Production layering:**
```
Request → Bulkhead → TimeLimiter → CircuitBreaker → Retry → call → Fallback
```

---

## Q6: High Availability Design

**Memory Hook:** No SPOF → Redundancy → Failover → Multi-Region

> "I design for high availability by ensuring no single point of failure across any layer.
>
> Infrastructure: deploy across multiple availability zones, load balance across them. If one zone fails, traffic routes to healthy zones automatically.
>
> Application: stateless services + Kubernetes health checks + auto-healing. Unhealthy pods are replaced without manual intervention.
>
> Data: replication — read replicas in Oracle, replica shards in OpenSearch. Data available even if primary fails.
>
> Failover: automatic within region. For critical systems, active-active multi-region with global load balancing.
>
> Trade-off: multi-region adds cost, cross-region data consistency complexity, and latency. I justify it based on RTO/RPO requirements — not every system needs multi-region."

---

## Q7: Data Consistency in Distributed Systems

**Memory Hook:** Correctness → Strong | Scale → Eventual | Patterns → Saga/Outbox

> "I apply consistency based on the criticality of the operation, not uniformly.
>
> For financial transactions or critical clinical decisions — strong consistency. Correctness cannot be compromised.
>
> For analytics, notifications, and reporting — eventual consistency. Slightly stale data is acceptable if throughput and availability are higher.
>
> For cross-service workflows — Saga pattern. For reliable event publishing — Outbox pattern. Idempotency across all async operations.
>
> The CAP theorem is real: in distributed systems, during network partition you choose between consistency and availability. I make this choice explicitly per use case, not by accident."

---

## Q8: Notification System Design

**Memory Hook:** Event → Queue → Processor → Channel → Retry → Status

**Architecture:**
```
Business Event (order placed, patient alert triggered)
    │
    ▼
Kafka Topic (notification-events)
    Producer includes: correlation ID, event type, recipient, channel preference
    │
    ▼
Notification Service (consumer)
    Determines channel: email / SMS / push / in-app
    Sends via channel provider
    Updates status: PENDING → SENT / FAILED
    │
    ├── Success: publish to status topic with correlation ID
    └── Failure: retry 3x with exponential backoff → DLQ
    
Producer tracking:
    → Subscribe to status topic with correlation ID
    → OR poll GET /notifications/{correlationId}/status
```

**Key design decisions:**
- At-least-once delivery with idempotent processing — not exactly-once (too complex, unnecessary)
- Rate limiting per channel provider — email vendors have rate caps
- User preferences: opt-in/opt-out stored in a preferences service
- DLQ for permanent failures — ops reviews daily, replay after fix

---

## Q9: 12-Factor App Principles

**Memory Hook:** Stateless → Config → Portable → Observable

| Factor | What It Means | Why It Matters |
|--------|--------------|----------------|
| Stateless processes | No session state in app layer | Enables horizontal scaling — any pod serves any request |
| Config via environment | No hardcoded values | Same codebase runs dev/staging/prod without changes |
| Logs as streams | Write to stdout | Centralized to Splunk/ELK automatically |
| Backing services as attached resources | DB, cache, queue are external | Easy to swap, easy to test |
| Dev/prod parity | Same environments | No "works on my machine" surprises |

**Real example:** All our microservices at Cerner follow 12-factor — config via environment variables managed by Kubernetes secrets, logs streamed to Splunk via FluentBit, stateless pods that auto-scale with HPA.

---

## QUICK REFERENCE — PATTERN DEFINITIONS

| Pattern | One-Line Definition | When to Use |
|---------|-------------------|-------------|
| SAGA | Distributed transaction using compensating operations | Multi-service workflows where one DB transaction cannot span all |
| Outbox | Write event to DB in same transaction as state change | Guarantee Kafka publish even if service crashes |
| Circuit Breaker | Stop calling failing service, allow recovery | Dependency that fails consistently |
| Bulkhead | Isolate thread pools per downstream | Prevent one slow service from starving others |
| Idempotency | Same operation multiple times = same result | Any retry-able operation |
| CQRS | Separate read and write models | High read/write ratio with different access patterns |
| Event Sourcing | State = sequence of events, not current snapshot | Audit requirements, time-travel queries |
| Blue-Green | Two live environments, cut traffic over | Low-risk major releases with instant rollback |
| Canary | Deploy to small % first, expand if healthy | Gradual feature rollout with real-world validation |
| Partitioning | Split data within one DB instance | Query performance on large tables |
| Sharding | Split data across multiple DB instances | Horizontal scale beyond single instance limits |

---

*File 4 of 6 — System Design & Architecture*
