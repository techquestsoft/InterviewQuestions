# TJX Engineering Manager Interview Prep
## File 2 of 2 — Technical: System Design, Architecture and Engineering

> **Role:** IT Engineering Manager — Global Supply Chain, Retail Distribution
> **Panel:** Senior Engineering Manager + Principal Engineer
> **Stack:** Java Microservices, Azure/AKS, Manhattan WMS, SAFe Agile

---

## HOW TO USE THIS FILE

The Principal Engineer will lead the technical questions. They want to see that you can:
- Clarify goals and constraints before jumping to solutions
- Make architecture decisions with explicit reasoning
- Go deep on distributed systems patterns
- Speak credibly about observability, resiliency, and data quality
- Connect technical choices to business outcomes

---

## DOMAIN 1 — System Design

---

### Q1: Design a Real-Time Inventory Tracking System for a Distribution Center

> This is the most likely system design question for this role. Lead with clarification — do not jump to architecture first.

---

#### Opening Statement

> "Before jumping into the design, I want to spend two or three minutes clarifying goals, success metrics, constraints, and NFRs — because the architecture choices flow directly from these. Is that okay?"

This signals EM-level thinking. Most candidates skip straight to boxes and arrows.

---

#### Step 1 — Clarify the Goal

Design a real-time inventory tracking system for TJX distribution centers that provides accurate, up-to-date stock visibility across DC zones — supporting pick, pack, and ship workflows and feeding downstream systems like Manhattan WMS and Oracle ERP.

**Clarifying questions to ask the interviewer:**

- What is the scale? How many DCs, SKUs per DC, scan events per second at peak?
- What does real-time mean here — sub-second for pickers, or near-real-time for reporting?
- Who are the downstream consumers — Manhattan WMS, replenishment system, store allocation, all three?
- Is this greenfield or integrating with existing Manhattan WMS?
- Any compliance requirements — SOX audit trail, GDPR for EU DCs, data residency?

**On SKUs and TJX-specific scale:**

> A SKU (Stock Keeping Unit) is a unique identifier for every distinct product variant.
> Nike T-Shirt + Blue + Medium = one SKU. Nike T-Shirt + Blue + Large = a different SKU.
> Every color, size, and style variation gets its own SKU, tracked independently.

TJX is an off-price retailer — opportunistic buying means high SKU churn:
- Standard retailer: 10,000–50,000 stable SKUs per DC
- TJX reality: 100,000–500,000+ active SKUs per DC, changing frequently

This changes the caching strategy significantly. You cannot cache all SKUs in Redis and expect them to stay fresh — you need LRU eviction, a smarter lookup layer, and a plan for SKU master data synchronization when new products arrive.

---

#### Step 2 — Define Success Metrics

| Metric | Target |
|--------|--------|
| Scan-to-system latency | < 500ms (p99) |
| Inventory accuracy vs physical count | > 99.5% |
| System availability | 99.9% |
| Current stock query response | < 100ms |
| End-to-end event processing lag | < 2 seconds |
| Duplicate scan handling | 100% idempotent |

---

#### Step 3 — Identify Constraints

**Technical constraints:**
- Must integrate with Manhattan WMS — cannot replace it
- Barcode scanners are edge devices — DC network can drop in warehouse zones
- Peak load during inbound receiving shifts is 3 to 5x normal volume
- Oracle ERP is the financial system of record

**Business constraints:**
- DC operations cannot be halted for a migration
- Multiple DC zones may have different scanner hardware generations
- Global DCs across NA, EU, AU — timezone and data residency considerations

**Compliance constraints:**
- SOX compliance requires an immutable inventory audit trail
- GDPR applies to EU DC data
- Data retention policy for all scan events

---

#### Step 4 — Define NFRs

| NFR | Requirement | Reasoning |
|-----|-------------|-----------|
| Scalability | Handle 10x peak scan volume | Black Friday, inbound surges |
| Reliability | Zero scan event loss | Audit and accuracy requirement |
| Idempotency | Handle scanner retransmissions | Network drops cause duplicate sends |
| Availability | 99.9% minimum | DC operations depend on this system |
| Auditability | Every stock movement traceable | SOX compliance |
| Latency | Picker confirmation < 500ms | Operational throughput |

---

#### Step 5 — Identify the Levers

> "I will structure the solution around five control points in the inventory lifecycle."

| Lever | Core Challenge | Desired Outcome |
|-------|---------------|-----------------|
| Scan Ingestion | Network drops, duplicate scans, high volume | Reliable, idempotent event capture |
| Inventory State Management | Multiple zones updating same SKU concurrently | Accurate, conflict-free stock levels |
| Reservation and Fulfillment | Pick task conflicts, potential overselling | Consistent reservation across pickers |
| Real-Time Query Layer | WMS and replenishment need current stock | Sub-100ms read latency |
| Audit and Compliance | SOX requirements, shrinkage investigation | Immutable, queryable event history |

---

#### Step 6 — Solution Options

**Option A — Monolithic WMS Extension**
- Extend Manhattan WMS to handle real-time event tracking
- Fast to implement, no new infrastructure
- Does not scale — WMS is not designed for high event throughput
- Rejected for peak load and NFR reasons

**Option B — Event-Driven Layer on top of WMS (Recommended)**
- Lightweight event ingestion layer above WMS
- Real-time read layer via Redis decoupled from WMS writes
- Balances integration reality with performance requirements
- Manhattan WMS remains the system of record

**Option C — Full Event Sourcing and CQRS**
- Complete event store, WMS becomes a downstream consumer
- Maximum flexibility and auditability
- High complexity and long timeline — not right for Phase 1
- Reserved for Phase 3 analytics evolution

**Decision:**
> "Start with Option B. Preserve Manhattan WMS as system of record, add an event-driven ingestion and real-time query layer on top. Evolve toward Option C for analytics as scale demands it."

---

#### Step 7 — Architecture

```
DC Floor (Warehouse Zones A, B, C...)
    │
    │  Barcode scanners / RFID readers / handheld devices
    ▼
Edge Gateway Service              ← deployed per DC zone
    │  Buffers scans if network drops (10K event local buffer)
    │  Assigns idempotency key to each scan
    │  Lightweight validation: valid SKU format, known zone ID
    │
    │  Kafka Topic: scan-events
    ▼
Scan Processing Service           ← core processing engine
    │  Checks idempotency key against Redis dedup cache
    │  Enriches event with SKU master data
    │  Classifies event: RECEIVE / PICK / PACK / SHIP / ADJUST
    │
    ├──► Inventory State Service   (Azure SQL — transactional writes)
    │       Owns current stock levels per SKU per zone
    │       Optimistic locking prevents concurrent pick conflicts
    │       Publishes INVENTORY_UPDATED events on each change
    │
    ├──► Real-Time Cache           (Redis — sub-100ms reads)
    │       Current stock levels for query layer
    │       Updated on every INVENTORY_UPDATED event
    │       LRU eviction for high SKU churn
    │       Manhattan WMS picker allocation queries here
    │
    ├──► Audit Event Store         (Cosmos DB / S3 — append only)
    │       Immutable scan event history
    │       SOX compliance and shrinkage investigation
    │       Never updated — only appended
    │
    └──► Manhattan WMS Sync Service
            Keeps WMS in sync with event stream
            Handles WMS API rate limits gracefully
            Dead letter queue for failed WMS sync attempts
                │
                ▼
         Oracle ERP               System of record for financials
         Replenishment System     Triggered by low-stock events
         Azure Synapse            Analytics and reporting layer
```

---

#### Step 8 — Key Design Decisions with Code

**Idempotency at scan level — prevent double-decrement:**
```java
// Edge gateway stamps every scan with a composite unique key
ScanEvent event = ScanEvent.builder()
    .idempotencyKey(deviceId + ":" + scanTimestamp + ":" + barcode)
    .skuId(barcode)
    .zoneId(zoneId)
    .eventType(PICK)
    .operatorId(operatorId)
    .build();

// Scan Processing Service checks before processing
if (redisDedup.exists(event.getIdempotencyKey())) {
    log.info("Duplicate scan ignored: {}", event.getIdempotencyKey());
    return; // acknowledge to scanner as success — silent idempotent skip
}
// Process and mark as seen with 24-hour TTL
redisDedup.set(event.getIdempotencyKey(), "seen", Duration.ofHours(24));
```

**Optimistic locking for concurrent pick conflicts:**
```java
@Transactional
void processPick(String skuId, String zoneId, int qty) {
    InventoryRecord record = repo.findBySkuAndZone(skuId, zoneId);

    if (record.getAvailableQty() < qty) {
        throw new InsufficientStockException(skuId, zoneId, qty);
        // Picker receives "insufficient stock" on handheld — accurate
    }

    record.setAvailableQty(record.getAvailableQty() - qty);
    record.setVersion(record.getVersion() + 1);  // optimistic lock version bump
    repo.save(record);
    // If two pickers process same SKU simultaneously:
    // Second save throws OptimisticLockException
    // Caller retries → reads updated record → may now see insufficient stock
    // Correct outcome — no oversell
}
```

**Edge buffering for DC network drops:**
```
Normal flow:
  Scanner → Edge Gateway → Kafka → Scan Processing   [real-time]

During network drop:
  Scanner → Edge Gateway [buffers events locally up to 10K]
                         [network restored]
                         → Kafka → Scan Processing   [ordered replay]

Result: Zero scan loss even during DC network instability
        Operations continue on the floor uninterrupted
```

**Outbox pattern for guaranteed event publishing:**
```java
@Transactional  // Single atomic DB transaction covers both writes
void updateInventoryState(String skuId, String zoneId, int delta) {
    // Write state change to inventory table
    inventoryRepo.updateStock(skuId, zoneId, delta);

    // Write event to outbox table IN SAME TRANSACTION
    outboxRepo.save(OutboxEvent.builder()
        .eventType("INVENTORY_UPDATED")
        .payload(buildPayload(skuId, zoneId, delta))
        .status(PENDING)
        .build());
    // If service crashes here: both writes roll back atomically
    // No ghost events, no phantom state changes
}

// Separate outbox poller — runs every 100ms
@Scheduled(fixedDelay = 100)
void publishPendingEvents() {
    outboxRepo.findByStatus(PENDING).forEach(event -> {
        kafkaTemplate.send("inventory-events", event.getPayload());
        outboxRepo.updateStatus(event.getId(), PUBLISHED);
    });
    // Consumers must be IDEMPOTENT to handle rare duplicate publishes
}
```

---

#### Step 9 — Failure Scenarios

| Scenario | Impact Without Mitigation | Mitigation |
|----------|--------------------------|------------|
| Kafka cluster down | Scan processing pauses | Edge gateway buffers; DC operations continue; resync on recovery |
| Redis cache stale or down | Slow read queries | Cache miss falls through to Azure SQL; repopulate on read; latency increases but accuracy holds |
| WMS sync lag | WMS shows stale inventory | DLQ for failed syncs; Redis is live source of truth; alert fires if lag exceeds 5 minutes |
| Duplicate scans at peak | Double inventory decrement | Idempotency key blocks at Scan Processing; scanner receives success acknowledgment; inventory unchanged |
| Concurrent pick of last item | Oversell / picker conflict | Optimistic locking; second picker receives InsufficientStockException; reassigned by WMS |

---

#### Step 10 — Phased Execution (PI-Aligned for SAFe)

**Phase 1 — Foundation (PI 1, one DC pilot)**
- Edge Gateway with local buffering deployed to one DC
- Scan Processing Service with idempotency
- Real-time Redis cache with LRU eviction
- Basic inventory state service (Azure SQL)
- Measure latency and accuracy vs targets

**Phase 2 — WMS Integration (PI 2, five DCs)**
- Manhattan WMS Sync Service with DLQ
- Inventory State Service with optimistic locking
- Audit Event Store in Cosmos DB
- Observability: New Relic dashboards, Splunk log integration
- Rollout to five DCs with canary approach

**Phase 3 — Scale and Analytics (PI 3, full global rollout)**
- Multi-DC federation and cross-DC inventory visibility
- Azure Synapse reporting and analytics layer
- Automated replenishment trigger from low-stock events
- CQRS evolution for high-volume analytical queries
- Full global DC rollout

---

#### Risks and Mitigations

| Risk | Trade-off | Mitigation |
|------|-----------|------------|
| Scanner network drops in DC | Reliability vs complexity | Edge buffer with local persistence, ordered replay |
| Concurrent picks of last unit | Consistency vs throughput | Optimistic locking, picker retry with fresh read |
| WMS sync lag during high volume | Real-time vs integration reality | Decouple read layer (Redis) from WMS sync |
| Audit data volume growth | Storage cost vs compliance | Hot tier 90 days, cold tier S3 after that |
| Peak inbound volume (3–5x) | Cost vs capacity | AKS HPA on Scan Processing, Kafka partition scaling |
| High SKU churn invalidating cache | Cache hit rate vs accuracy | LRU eviction, SKU master data sync pipeline |

---

**Closing Statement:**
> "The core design principle: capture every scan reliably at the edge, process it exactly once in the core, serve current stock sub-100ms from cache, and sync to WMS asynchronously — so DC operations are never blocked by system latency or downstream failures."

---

### Q2: Design a WMS Fulfillment Flow Using SAGA and Outbox Patterns

---

#### The Problem — Distributed Transactions

Fulfilling a warehouse order touches multiple bounded contexts, each with its own database:

```
Inventory Service  → owns: inventory_reservations  (Azure SQL)
Warehouse Service  → owns: pick_tasks              (Azure SQL)
Shipping Service   → owns: shipments               (Cosmos DB)
Dispatch Service   → owns: dispatch_confirmations  (Azure SQL)
```

A single @Transactional annotation cannot span all four. If you try:

```java
// WRONG — this is a distributed transaction anti-pattern
@Transactional
void fulfillOrder(String orderId) {
    inventoryDB.reserve(sku, qty);           // DB 1 — committed
    warehouseDB.createPickTask(orderId);     // DB 2 — NOT covered
    shippingDB.createShipment(orderId);      // DB 3 — crashes here
    // DB 1 and DB 2 are committed with no matching shipment
    // Inventory reserved for an order that will never ship
}
```

---

#### SAGA Pattern — Orchestration Style

The Saga Orchestrator coordinates each step and triggers compensating transactions on failure:

```
Order Fulfillment Orchestrator
        │
        ▼
Step 1: Reserve Inventory
        SUCCESS → proceed to Step 2
        FAILURE → end saga, return error to order service

Step 2: Create Pick Task
        SUCCESS → proceed to Step 3
        FAILURE → COMPENSATE: Release inventory reservation

Step 3: Generate Shipment
        SUCCESS → proceed to Step 4
        FAILURE → COMPENSATE: Cancel pick task
                           → Release inventory reservation

Step 4: Confirm Dispatch
        SUCCESS → SAGA COMPLETE
        FAILURE → COMPENSATE all three prior steps
```

Each compensation is a real business operation:

```java
// Compensation for Step 1 — Release inventory reservation
void releaseInventoryReservation(String reservationId) {
    inventory.updateStatus(reservationId, RELEASED);
    outboxRepo.save(new OutboxEvent("INVENTORY_RELEASED", reservationId));
}

// Compensation for Step 2 — Cancel pick task
void cancelPickTask(String pickTaskId) {
    pickTask.updateStatus(pickTaskId, CANCELLED);
    // Notify warehouse floor system of cancellation
    outboxRepo.save(new OutboxEvent("PICK_TASK_CANCELLED", pickTaskId));
}
```

---

#### Are These Steps Transactional or Event-Based?

They are hybrid — each step has a transactional core (DB write for accuracy) plus an event emitted to notify other services.

| Step | Transactional Core | Event Emitted |
|------|-------------------|---------------|
| Reserve Inventory | Strong ACID — prevents double reservation | INVENTORY_RESERVED via Outbox |
| Create Pick Task | Task must exist in DB before picker can act | PICK_TASK_CREATED via Outbox |
| Generate Shipment | Shipment ID needed for carrier and label | SHIPMENT_GENERATED via Outbox |
| Confirm Dispatch | Eventual — scanner is source of truth | Primarily event-driven (scanner → Kafka) |

---

#### Outbox Pattern — Why SAGA Alone Is Not Enough

A crash between the DB write and the Kafka publish creates a ghost state:

```java
@Transactional
void reserveInventory(String sku, int qty) {
    inventoryRepo.save(new Reservation(sku, qty, RESERVED)); // Step A: committed
    kafkaTemplate.send("inventory-events", new ReservedEvent()); // Step B: crash here
    // DB committed, Kafka message never sent
    // Orchestrator times out → triggers compensation → releases reservation
    // But the reservation record still exists in DB → ghost reservation
}
```

Outbox pattern fixes this by writing the event to the DB in the same transaction as the state change:

```java
@Transactional  // One atomic transaction — both writes succeed or both fail
void reserveInventory(String sku, int qty) {
    inventoryRepo.save(new Reservation(sku, qty, RESERVED));

    outboxRepo.save(OutboxEvent.builder()
        .eventType("INVENTORY_RESERVED")
        .payload(buildReservationPayload(sku, qty))
        .status(PENDING)
        .build());
    // Crash here → both writes roll back → clean state
}

// Outbox poller runs separately, publishes pending events to Kafka
@Scheduled(fixedDelay = 100)
void publishPendingEvents() {
    outboxRepo.findByStatus(PENDING).forEach(event -> {
        kafkaTemplate.send("inventory-events", event.getPayload());
        outboxRepo.updateStatus(event.getId(), PUBLISHED);
    });
    // Kafka down → events stay PENDING → retry on next poll
    // Consumers must handle duplicates idempotently
}
```

---

## DOMAIN 2 — Architecture Depth

---

### Q3: How do you approach technology lifecycle management?

**Answer:**

I maintain a technology radar organized into four quadrants — Adopt, Trial, Assess, Hold — reviewed quarterly with the team.

**Per technology I track:**

| Technology | Current State | Action | Owner | Deadline |
|-----------|--------------|--------|-------|---------|
| Java 11 | In production | Upgrade to Java 21 LTS | Platform team | Q3 |
| Spring Boot 2.x | In production | Migrate to 3.x before EOL | Each service team | Q4 |
| Kubernetes 1.26 | In production | AKS auto-upgrade configured | DevOps | Automated |
| Log4j | Scanning for old versions | CVE-driven replacement | Security team | Immediate |

**Dependency hygiene in CI pipeline:**
The build fails if any dependency has a critical CVE. Engineers cannot defer security patches by forgetting to check — the pipeline enforces it.

**For major migrations like Java 8 to 17:**
I run migrations as a dedicated parallel track with reserved capacity — not as a side task competing with feature sprints. Feature work and migration work sharing the same engineers is how migrations fail.

**Business case to stakeholders:**
> "Running EOL technology is a hidden liability. It will not fail today. But when it does fail, the blast radius and recovery cost are far larger than a planned migration. The question is not whether to pay the cost — it is whether to pay it on our terms or on an incident's terms."

---

### Q4: How do you design for observability in a microservices system?

**Memory Hook:** Logs + Metrics + Traces + Events = Complete Observability

**Answer:**

Observability is built on three pillars — logs, metrics, and traces. But there is a fourth pillar that most teams miss: events. Without events you cannot answer the most important question when something goes wrong: what changed just before this started?

| Pillar | What it answers | Your stack |
|--------|----------------|-----------|
| Logs | What happened, with context | Splunk |
| Metrics | How the system is behaving over time | New Relic |
| Traces | Why a specific request was slow or failed | New Relic APM |
| Events | What changed just before the problem started | New Relic deployment markers |

---

**Three levels of observability for WMS:**

**Business level:**
- Orders picked per hour versus target throughput
- Inventory sync lag from WMS to system of record
- Shipment confirmation rate and dispatch time

**Service level:**
- p50, p95, p99 latency per endpoint
- Error rate per service per endpoint
- Kafka consumer lag per topic and consumer group

**Infrastructure level:**
- Pod CPU and memory versus limits (AKS)
- Database connection pool utilization
- JVM garbage collection pause time for Java services

---

**SLO definition:**
Inventory query API — 99.9% of requests respond within 200ms. Alert fires at 99.5% — giving time to investigate before the SLO is breached rather than after.

**Critical discipline:** Every alert must be actionable. If an alert fires and the engineer's response is "nothing to do, it self-resolved" — that alert needs its threshold adjusted or it needs to be deleted. Alert fatigue is how on-call culture breaks down.

---

#### Events — The Fourth Pillar Explained

Events answer: "What changed just before this problem started?"

**Without events:**
```
New Relic alert: p99 latency spike at 14:32
Logs: slow database queries
Metrics: DB CPU jumped at 14:31
Question: WHY did it jump?
Without events: two-hour war room
With events: deployment marker shows v2.3.1 was pushed at 14:30
Root cause found in eight minutes
```

**Three types of events to capture:**

**1. Deployment events** — every service version push to production:
```json
{
  "type": "DEPLOYMENT",
  "service": "inventory-service",
  "version": "v2.3.1",
  "previous_version": "v2.3.0",
  "timestamp": "2025-04-30T14:30:00Z",
  "environment": "production"
}
```
New Relic renders these as vertical markers on every chart — instant visual correlation.

**2. Configuration change events** — feature flags toggled, config values changed, scaling policies updated:

In WMS context:
- Kafka consumer thread count changed from 10 to 50 → sudden CPU spike that looks like a bug
- Feature flag "new-picking-algorithm" enabled for 10% of DCs → error rate climbs in those DCs
- Circuit breaker threshold changed → services open circuit more aggressively

**3. Traffic spike events** — sudden volume changes that help you distinguish "our code got slower" from "we received 3x normal traffic":

In WMS context:
- Black Friday inbound surge — expected, do not page on-call
- Upstream order system replayed 50K orders — unexpected, needs investigation
- Manhattan WMS sent duplicate sync events — looks like system slowness, actually double event volume

**Full timeline with events:**
```
14:28 — normal latency (50ms)
14:30 — [DEPLOYMENT: inventory-service v2.3.1]
14:31 — [CONFIG CHANGE: DB connection pool 20 → 50]
14:32 — latency spikes to 800ms
14:33 — [ALERT FIRED: p99 > 500ms]
14:45 — [DEPLOYMENT: rollback to v2.3.0]
14:46 — latency returns to 55ms

Root cause: v2.3.1 + connection pool change
Time to identify: 8 minutes instead of 2 hours
```

---

### Q5: How do you handle a major production incident in a distributed system?

**Memory Hook:** Mitigate First → Communicate Every 15 Minutes → Blameless Postmortem → Evidence-Based Actions

**Answer:**

I follow a structured SEV framework with clear roles and a strict communication cadence.

**First 15 Minutes — Stabilize:**
- Declare severity level (SEV1 through SEV3)
- Assign incident commander — one person owns the incident, not a committee
- Open a dedicated war room channel
- Assess customer impact — how many users affected, what is the business impact

**Mitigation Before Root Cause:**
Restore service first — rollback the deployment, flip the feature flag, fail over to backup. Customers cannot wait for root cause analysis. We debug after we stop the bleeding.

**Communication Cadence:**
Stakeholders receive an update every 15 minutes during an active incident — even if the update is "still investigating, no change." Silence is worse than uncertainty. Stakeholders who do not hear anything assume the worst.

**Post-Incident:**
Blameless postmortem within 48 hours. Five whys to identify the real root cause, not the surface cause. Action items must be specific, owned, and time-bound:
- Wrong: "Add more monitoring"
- Right: "Add p99 latency alert on inventory-service Kafka consumer by Friday — owner: [name]"

**Real Example:**
At Cerner, our readmission risk pipeline had a data processing failure affecting three health systems simultaneously. I led the incident. We mitigated in 47 minutes by replaying Kafka from the last committed offset. The postmortem surfaced a missing idempotency check that caused duplicate processing under retry. Fixed within the same sprint. Incident rate from that pipeline dropped to zero for the following six months.

---

## DOMAIN 3 — Distributed Systems Concepts

---

### Q6: Explain partitioning and sharding — OpenSearch versus PostgreSQL

**Core Concept:** Both split large datasets into smaller chunks for performance and scalability. The difference is where and how.

---

**OpenSearch — Sharding by Default**

Every index is automatically divided into primary shards and replica shards when created.

```json
PUT /patient_records
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  }
}
```

Routing formula: `shard = hash(document_id) % number_of_primary_shards`

Ten million patient records distribute roughly evenly across three shards on three different nodes. A query fans out to all shards in parallel, results are merged — fast horizontal throughput.

Key traits:
- Sharding is automatic — you configure it at index creation
- Shard count is fixed at creation — hard to change later without reindexing
- Replicas add fault tolerance, not additional primary storage

---

**PostgreSQL — Declarative Partitioning**

You manually define how a table is split. The DB engine routes reads and writes transparently.

```sql
-- Parent partitioned table
CREATE TABLE orders (
    order_id   SERIAL,
    order_date DATE NOT NULL,
    amount     NUMERIC,
    customer   TEXT
) PARTITION BY RANGE (order_date);

-- Individual partitions
CREATE TABLE orders_2023 PARTITION OF orders
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE orders_2024 PARTITION OF orders
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
```

Query with `WHERE order_date = '2024-06-15'` → PostgreSQL's partition pruning scans only orders_2024 — not the full 10 million row table.

Three strategies: RANGE (dates, IDs), LIST (country, status), HASH (even distribution with no natural column).

---

**Can RDBMS Shard? Can OpenSearch Partition? Both, yes.**

The terms describe different layers of the same concept:

| | Partitioning | Sharding |
|--|-------------|---------|
| Location | Usually within one machine | Across multiple machines |
| Who decides | DB engine based on schema rules | Hash function or routing key |
| Primary goal | Query pruning, manageability | Horizontal scale, throughput |
| Relationship | Sharding is distributed partitioning | Partitioning can become sharding |

**RDBMS sharding options:**
- Citus extension for PostgreSQL — distributes tables across nodes automatically
- Vitess — sharding middleware for MySQL used by YouTube and GitHub
- Application-level sharding — app decides which DB server to connect to based on a key

**OpenSearch partitioning options:**
- Index-per-tenant — one OpenSearch index per customer, query only relevant indices
- Time-based indices — orders-2024, orders-2025 — prune by index, not by scan
- Custom routing keys — force related documents to the same shard

**Rule of thumb:**
- Partitioning = splitting data on one machine
- Sharding = splitting data across many machines
- Sharding is partitioning at distributed scale

---

### Q7: Is "Scaling always involves trade-offs between consistency and performance" correct?

**Short answer:** Partially true but an oversimplification that a senior engineer should push back on.

**Where it holds true:**
In distributed systems the CAP theorem forces a real choice. When network partitions occur — which are inevitable at distributed scale — you must choose between consistency (all nodes see the same data) and availability (system always responds). Kafka, Cassandra multi-region, OpenSearch read replicas — these all involve genuine consistency trade-offs for throughput gains.

**Where it breaks down:**

Scaling stateless services has no consistency cost. Adding AKS pods for a REST API, scaling a CDN for static assets, adding connection pooling — these are pure performance gains with zero consistency trade-off. There is no shared state to be inconsistent.

Some scaling patterns have minimal cost. A read replica with a short cache TTL introduces only tolerable lag. Horizontal scaling of stateless microservices — like your Care Management APIs on AKS — has essentially zero consistency trade-off.

Modern databases challenge the assumption. Google Spanner and CockroachDB provide strong consistency at global distributed scale using synchronized clocks and consensus protocols. They exist specifically to disprove the "you must trade consistency for scale" assumption.

The trade-off is technically consistency versus availability, not consistency versus performance. Performance is a consequence — not the direct trade-off axis.

**More accurate statement:**
> "Scaling distributed, stateful systems often involves trade-offs between consistency, availability, and latency. But stateless scaling and modern distributed databases can avoid or significantly minimize these trade-offs."

---

### Q8: What is backpressure handling and how do you apply it?

**Simple Analogy:**
A water tank fills at 100 litres per minute. The pipe leads to a bucket that empties at 20 litres per minute. Without flow control the bucket overflows. Backpressure is the mechanism that tells the tank to slow down.

In software: Producer sends 10,000 messages per second. Consumer processes 1,000 per second. Without backpressure the buffer fills, memory exhausts, and the system crashes — or worse, silently drops messages.

---

**The Five Core Strategies:**

**1. Rate Limiting — Reject early at the gate**

Do not let more requests enter than you can handle downstream.

```java
RateLimiter rateLimiter = RateLimiter.of("inventory-api",
    RateLimiterConfig.custom()
        .limitForPeriod(1000)                    // max 1000 calls per second
        .limitRefreshPeriod(Duration.ofSeconds(1))
        .timeoutDuration(Duration.ofMillis(100))
        .build());

rateLimiter.executeSupplier(() -> inventoryService.checkStock(orderId));
// Caller exceeds limit → RequestNotPermitted → 429 Too Many Requests
// Clean, fast failure — no thread blocking
```

When to use: Protecting internal services from external traffic spikes. API Gateway or NGINX ingress layer.

**2. Circuit Breaker — Stop calling a failing service**

Retrying a consistently failing service makes the problem worse. The circuit breaker detects failure patterns and stops calls immediately, giving the downstream service time to recover.

```java
CircuitBreaker cb = CircuitBreaker.of("inventory-service",
    CircuitBreakerConfig.custom()
        .failureRateThreshold(50)                // open if 50% of calls fail
        .slowCallRateThreshold(80)               // open if 80% of calls are slow
        .slowCallDurationThreshold(Duration.ofSeconds(2))
        .waitDurationInOpenState(Duration.ofSeconds(30))
        .slidingWindowSize(10)                   // evaluate last 10 calls
        .build());
```

Three states:
- CLOSED — normal operation, calls flow through
- OPEN — too many failures, all calls fail immediately (fast fail)
- HALF-OPEN — one test call allowed through to check recovery

**3. Queue-Based Buffering — Absorb the spike**

Put Kafka between producer and consumer. Consumer pulls at its own pace regardless of producer volume.

```java
// Producer — fire and forget
kafkaTemplate.send("scan-events", scanId, scanPayload);

// Consumer — processes at comfortable rate
@KafkaListener(topics = "scan-events", groupId = "inventory-processor")
public void processScan(ScanEvent event) {
    inventoryService.updateStock(event);
    // Consumer group scales out if lag grows
}
```

Kafka consumer lag is your backpressure signal — monitor it in New Relic. Alert when lag exceeds your defined threshold.

**4. Reactive Streams — Demand-based flow control**

The consumer explicitly tells the producer how many items it can handle. The producer respects that signal.

```java
Flux.fromIterable(millionScanEvents)
    .onBackpressureBuffer(1000)              // buffer up to 1000 in memory
    .flatMap(event ->
        inventoryService.processEvent(event),
        16)                                  // max 16 concurrent calls
    .subscribe(
        result -> metrics.increment("processed"),
        error  -> log.error("failed: {}", error)
    );
```

**5. Bulkhead — Isolate failures to a pool**

Limit how many threads each downstream service can consume. One slow service cannot starve threads away from other services.

```java
Bulkhead openSearchBulkhead  = Bulkhead.of("opensearch",
    BulkheadConfig.custom().maxConcurrentCalls(20).build());
Bulkhead millenniumBulkhead  = Bulkhead.of("millennium",
    BulkheadConfig.custom().maxConcurrentCalls(20).build());

// Without bulkhead: OpenSearch slow → all 200 threads stuck → Millennium starved
// With bulkhead: OpenSearch slow → 20 threads affected → 180 threads still available
```

---

**Production Layering — All Patterns Together:**

```java
return Decorators
    .ofSupplier(() -> inventoryService.checkStock(skuId))
    .withBulkhead(inventoryBulkhead)       // limit concurrency
    .withTimeLimiter(timeLimiter)          // do not wait forever
    .withCircuitBreaker(circuitBreaker)    // stop if consistently failing
    .withRetry(retry)                      // retry transient errors
    .withFallback(
        Arrays.asList(TimeoutException.class, CallNotPermittedException.class),
        throwable -> getFallbackStock(skuId)  // return something useful
    )
    .get();
```

Order matters: Bulkhead → TimeLimiter → CircuitBreaker → Retry → actual call → Fallback if all fail.

---

## DOMAIN 4 — Observability Stack

---

### Q9: Our stack is AKS, Splunk, and New Relic. Where does Jaeger fit?

**Short answer:** Jaeger is not needed. New Relic already provides distributed tracing. Adding Jaeger would be duplication and operational overhead with no benefit.

**What your stack already covers:**

| Tool | Role |
|------|------|
| AKS | Infrastructure health, pod metrics, resource usage |
| Splunk | Log aggregation, deep search, long-term retention |
| New Relic | APM, dashboards, alerting, AND distributed tracing |

**Setting up New Relic distributed tracing — zero code change required:**
```yaml
containers:
  - name: inventory-service
    image: inventory-service:2.1.0
    env:
      - name: NEW_RELIC_LICENSE_KEY
        valueFrom:
          secretKeyRef:
            name: newrelic-secret
            key: license-key
      - name: NEW_RELIC_APP_NAME
        value: "inventory-service"
      - name: NEW_RELIC_DISTRIBUTED_TRACING_ENABLED
        value: "true"
    command: ["java", "-javaagent:/newrelic/newrelic.jar", "-jar", "app.jar"]
```

New Relic auto-instruments HTTP calls, DB queries, and Kafka publish/consume across all services.

**The key integration — linking New Relic traces to Splunk logs via traceId:**

```xml
<!-- logback.xml — New Relic agent injects trace context into MDC automatically -->
<pattern>
  %d{ISO8601} [%thread] %-5level %logger
  traceId=%X{trace.id} spanId=%X{span.id}
  service=inventory-service
  - %msg%n
</pattern>
```

Every log line now carries the traceId. In Splunk:
```
index=prod-aks traceId="abc123xyz"
| sort _time
| table _time, service, level, message
```

This gives you every log line, from every service, for one specific request — in chronological order.

**Practical debugging flow with your stack:**
```
1. New Relic alert fires
2. Open distributed trace → find slow or failed span → copy traceId
3. Splunk: search traceId → full log story across all services
4. AKS: kubectl logs <pod> if issue is infrastructure-level
```

**When Jaeger would make sense:** Open-source requirement, on-premises trace storage mandate, no New Relic APM license. None of these apply to your current stack.

---

## DOMAIN 5 — Data Quality and Integrity

---

### Q10: How do you ensure data integrity and quality in the Millennium to S3 pipeline?

**Pipeline:**
```
Millennium (Oracle) → Crawl/Extract → Aggregation + Oncology Mappings
                    + EPMI Association → Patient Record → S3
```

Issues can enter at every stage. Quality checks are needed at all four levels.

---

**Level 1 — Data Integrity (Did data survive the move intact?)**

At extraction — row count reconciliation:
```python
millennium_count = oracle_query(
    "SELECT COUNT(*) FROM patient_encounters WHERE extracted_date = :date")
raw_layer_count = count_records_in_raw_layer(date)

if millennium_count != raw_layer_count:
    raise DataIntegrityException(
        f"Row count mismatch: source={millennium_count}, raw={raw_layer_count}")
```

Checks to implement:
- Row count match between Millennium and raw layer
- Checksum validation — bytes must match after transit
- Primary key uniqueness — no duplicate patient_id or encounter_id
- Null checks on PKs — patient_id and MRN can never be null
- Date range coverage — no gaps in extraction window

At transformation — record survival check:
```python
pre_transform_count  = count_raw_records()
post_transform_count = count_transformed_records()
dropped = pre_transform_count - post_transform_count

if dropped > acceptable_threshold:
    alert(f"{dropped} records lost during transformation — investigate")

# Every drop must be intentional and logged with reason
log_dropped_records(reason="no_epmi_match", record_ids=[...])
```

At S3 load:
```python
assert records_sent == count_s3_objects(prefix=f"patients/{date}/"), "S3 write incomplete"
assert verify_s3_checksums(batch_id), "Data corruption detected in S3"
```

---

**Level 2 — Data Validity (Does data conform to business rules?)**

Patient identity validation:
```python
def validate_patient_identity(record):
    if not record.mrn or not re.match(r'^MRN\d{8}$', record.mrn):
        flag("INVALID_MRN_FORMAT", record.patient_id)
    if record.dob > today:
        flag("DOB_IN_FUTURE", record.patient_id)
    if record.age > 150:
        flag("UNREALISTIC_AGE", record.patient_id)
    if record.gender not in ['M', 'F', 'U', 'O']:
        flag("INVALID_GENDER_CODE", record.patient_id)
```

Oncology mapping quality:
```python
def validate_oncology_mappings(record):
    for diagnosis in record.diagnoses:
        if not is_valid_icd10(diagnosis.code):
            flag("INVALID_ICD10_CODE", diagnosis.code)
        if is_cancer_diagnosis(diagnosis.code) and not record.staging:
            flag("MISSING_CANCER_STAGING", record.patient_id)

    mapped   = len([d for d in record.diagnoses if d.oncology_term])
    coverage = mapped / len(record.diagnoses)
    if coverage < 0.95:
        alert(f"Low oncology mapping coverage: {coverage:.1%}")
        # Expect 95%+ — below this indicates mapping pipeline degradation
```

EPMI association quality:
```python
def validate_epmi_association(record):
    if not record.epmi_id:
        flag("EPMI_UNLINKED", record)          # requires manual review
    if record.dob != epmi_record.dob:
        flag("EPMI_DOB_MISMATCH", record)      # possible wrong link
    if count_patients_with_epmi(record.epmi_id) > 1:
        flag("EPMI_DUPLICATE_LINK", record)    # two patients linked to same EPMI
```

---

**Level 3 — Data Consistency (Is data coherent across related entities?)**

Temporal consistency — healthcare data has strict time logic:
```python
def validate_temporal_consistency(record):
    for encounter in record.encounters:
        if encounter.discharge_date < encounter.admission_date:
            flag("DISCHARGE_BEFORE_ADMISSION", encounter.id)

        for procedure in encounter.procedures:
            if not (encounter.admission_date <= procedure.date <= encounter.discharge_date):
                flag("PROCEDURE_OUTSIDE_ENCOUNTER_WINDOW", procedure.id)
```

Referential integrity:
- Every encounter must link to a valid patient
- Every diagnosis must link to a valid encounter
- No orphaned records after transformation
- No dangling foreign key references

---

**Level 4 — Data Completeness (Is all expected data present?)**

Statistical anomaly detection — catches systemic issues that row-level checks miss:
```python
def run_statistical_checks(daily_batch):
    avg_daily_volume = get_30day_rolling_average()

    if daily_batch.count < avg_daily_volume * 0.7:
        alert("VOLUME_DROP: possible extraction failure or Millennium issue")
    if daily_batch.count > avg_daily_volume * 1.5:
        alert("VOLUME_SPIKE: possible duplicate extraction")

    null_rate = calculate_null_rate(daily_batch, field="oncology_term")
    if null_rate > historical_null_rate + 0.05:
        alert("NULL_RATE_INCREASED: mapping pipeline degradation?")
```

Idempotency and deduplication:
```python
def upsert_patient_record(record):
    existing = fetch_from_s3(patient_id=record.patient_id)
    if existing:
        if existing.checksum == record.checksum:
            skip("NO_CHANGE_DETECTED")
        else:
            update_with_version(record)       # changed — update and version
    else:
        insert_new(record)

# S3 key design for idempotency
s3_key = f"patients/{patient_id}/record.json"       # Overwrite-safe
# NOT: f"patients/{patient_id}/{timestamp}.json"    # Creates duplicates
```

---

**Quality Gates at Each Pipeline Stage:**

```
Millennium Oracle
      ↓
[EXTRACTION CHECKPOINT]
  Row count reconciliation
  PK null and duplicate check
  Date gap detection
      ↓
[TRANSFORMATION CHECKPOINT]
  Record survival count
  EPMI association validation
  Oncology mapping coverage
  Temporal consistency
  Referential integrity
      ↓
[LOAD CHECKPOINT]
  S3 write verification
  Checksum match
  Deduplication
  PHI completeness
      ↓
[POST-LOAD MONITORING]
  Statistical volume anomalies
  Null rate drift
  Mapping coverage KPI trends
```

**Metrics to Track in New Relic / Splunk:**

| Metric | Alert Threshold |
|--------|----------------|
| Extraction row count vs Millennium | Any mismatch — alert immediately |
| Record drop rate in transformation | > 1% |
| EPMI unlinked rate | > 0.5% |
| Oncology mapping coverage | < 95% |
| Daily volume vs 30-day average | < 70% or > 150% |
| S3 write success rate | < 100% |
| Pipeline end-to-end latency | > defined SLO |

---

## QUICK REFERENCE — Technical Concepts

| Concept | One-Line Definition |
|---------|-------------------|
| SKU | Stock Keeping Unit — unique ID for each product variant (color + size + style) |
| Sharding | Splitting data across multiple machines using a hash or routing key |
| Partitioning | Splitting data within one machine using range, list, or hash on a column |
| SAGA Pattern | Managing distributed transactions using compensating operations per step |
| Outbox Pattern | Writing event to same DB transaction as state change to guarantee publish |
| Circuit Breaker | Detects failure patterns, stops calls fast, allows recovery time |
| Bulkhead | Isolates thread pools per service so one slow service cannot starve others |
| Backpressure | Consumer signals to producer to slow down when overwhelmed |
| Idempotency | Same operation executed multiple times produces the same result |
| Optimistic Locking | Version-based conflict detection without holding a DB lock |
| Event Sourcing | Storing state as an ordered sequence of events rather than current snapshot |
| CQRS | Separate models for write (commands) and read (queries) operations |
| CAP Theorem | Distributed systems can guarantee at most 2 of: Consistency, Availability, Partition tolerance |
| Dead Letter Queue | Queue for messages that failed processing after all retry attempts |
| Partition Pruning | DB optimizer skipping irrelevant partitions based on query predicates |

---

*TJX Engineering Manager Interview — Technical: System Design, Architecture and Engineering*
*Companion file: TJX_People_ProjectManagement.md*
