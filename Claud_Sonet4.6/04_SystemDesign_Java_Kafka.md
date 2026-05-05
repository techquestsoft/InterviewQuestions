# Interview Prep — File 4 of 8
# System Design, Architecture, Java/Spring Boot & Kafka

> **Tailored for:** JPMorgan Chase — Senior Manager of Software Engineering, BBAO team. Stack focus: Java/Spring Boot microservices, REST APIs, Kafka event-driven architecture.

> **Rule 1:** Never start with architecture. Always: Goals → Metrics → Constraints → NFRs → Levers → Options → Architecture.
> **Rule 2:** Every design decision needs a trade-off. No trade-off = not senior-level thinking.
> **Rule 3:** Use real numbers from your experience — 5M weekly transactions, 110+ services, 500M patient records.

---

## CROSS-FILE INDEX

This file owns: design framework, distributed system patterns, monolith vs microservices, scaling, backpressure, HA, notification system, partitioning/sharding, Java/Spring Boot service patterns, Kafka producer/consumer/streaming patterns.
- Production reliability, observability, cloud services, Kubernetes, databases → File 06
- Data quality, ETL, data integrity → File 08
- GenAI architecture → File 07
- UI architecture (React/Angular) → File 05

---

## THE UNIVERSAL SYSTEM DESIGN OPENER

> "Before I jump into the design, I want to spend two to three minutes clarifying goals, success metrics, constraints, and NFRs — because every architecture decision flows from these. Is that OK?"

This single signal separates senior from junior. Junior candidates draw boxes immediately.

---

## DESIGN FRAMEWORK — APPLY TO EVERY QUESTION

```
Step 1: GOAL        — What problem? Who are the users?
Step 2: METRICS     — How do we measure success? (latency, throughput, accuracy)
Step 3: CONSTRAINTS — What cannot change? (existing systems, budget, compliance)
Step 4: NFRs        — Scalability, reliability, availability, security, idempotency
Step 5: LEVERS      — Break system into 4–5 control points
Step 6: OPTIONS     — A (simple), B (recommended), C (complex). Pick B with reasoning.
Step 7: ARCHITECTURE — Components, data flow
Step 8: FAILURES    — What breaks? Mitigation?
Step 9: TRADE-OFFS  — What was sacrificed? Alternatives?
Step 10: PHASES     — Build incrementally
```

---

## SECTION A — CORE DESIGN PHILOSOPHY

---

### Q1: How do you approach system design?

**Memory Hook:** Problem → Break → Trade-offs → Align → Outcome

> "I approach system design business-first, not technology-first.
>
> First, I clarify the problem — what we are building, who the users are, what scale we are targeting. Along with that, I identify key NFRs like latency, availability, throughput — these directly drive architectural decisions.
>
> Once the problem is clear, I break the system into logical layers — typically APIs, processing, and data — to identify responsibilities and bottlenecks.
>
> Then I evaluate architectural options. Whether a modular monolith is sufficient or we need microservices or event-driven design — based on domain boundaries, scaling needs, and team structure.
>
> Critically, I evaluate trade-offs. Scalability, maintainability, cost, and operational complexity. I do not over-engineer early.
>
> Finally, I ensure the design aligns with enterprise standards — security, observability, governance — so it is production-ready and compliant from day one."

---

### Q2: Monolith vs Microservices

**Memory Hook:** Team + Domain + Scale → Architecture Choice

> "I do not default to microservices. The decision depends on three factors: team structure, domain complexity, and scaling granularity needed.
>
> **Monolith** is right when the domain is unified, the team is small, change frequency is low, scale is modest. My BoA campaign tool served 1,000–2,000 internal users. Microservices would have added overhead with zero benefit.
>
> **Microservices** are right when different parts have genuinely different scaling needs and change rates. At Optum, the eligibility API and claims API had completely different load patterns. A monolith forces you to scale everything together.
>
> On 'can we scale a monolith behind a gateway?' — yes, you can scale horizontally. The problem is granularity. If eligibility is the bottleneck, you scale the entire monolith, paying for compute claims and benefits do not need. Kubernetes gives pod-level precision.
>
> My preference: start modular monolith, extract to microservices when domain boundaries are clear, team structure supports it, and observability is in place. Premature microservices is a common enterprise anti-pattern."

**Trade-off table:**
| | Monolith | Microservices |
|--|---------|--------------|
| Build speed | Fast initially | Slower — more infrastructure |
| Debugging | Easy — single codebase | Hard — distributed tracing required |
| Scaling | Coarse-grained | Fine-grained |
| Team independence | Low | High |
| Operational overhead | Low | High |
| Network latency | None | Real cost |

---

### Q3: 12-Factor App Principles

**Memory Hook:** Stateless → Config → Portable → Observable

| Factor | Meaning | Why |
|--------|---------|-----|
| Stateless processes | No session state in app layer | Horizontal scaling — any pod serves any request |
| Config via environment | No hardcoded values | Same codebase across dev/staging/prod |
| Logs as streams | Write to stdout | Centralized to Splunk via FluentBit |
| Backing services as attached | DB, cache, queue external | Easy to swap, easy to test |
| Dev/prod parity | Same environments | No "works on my machine" |

**Real example:** All Cerner microservices follow 12-factor — config via Kubernetes secrets, logs streamed to Splunk via FluentBit, stateless pods that auto-scale with HPA.

---

### Q4: What changes beyond architecture when system scales?

**Memory Hook:** Scale = Tech + Quality + Process + Governance

> "When systems scale, more than architecture needs to evolve.
>
> Technology side — architecture handles higher load, but engineering quality also matters. Stricter coding standards, automated testing, CI/CD quality gates.
>
> Operational maturity becomes critical. Observability — metrics, logs, tracing — plus clearly defined SLOs and alerting so issues are detected and resolved fast.
>
> Governance — release controls, approval workflows, audit trails, compliance checks. Especially in regulated environments like healthcare.
>
> Process and team structure — domain ownership, standardized practices across teams to avoid fragmentation.
>
> Real example at Cerner: as we scaled, we introduced CI/CD quality gates with security scans, defined SLOs with centralized observability, added CAB approvals and audit trails for production releases. System reliability and compliance improved without crushing delivery velocity."

---

## SECTION B — SCALING

---

### Q5: How do you design for scalability?

**Memory Hook:** Stateless → Distribute → Optimize → Async

> "Stateless services first — replicable and scalable via Kubernetes. Then distribute traffic via API gateway and load balancers.
>
> Optimize the data layer — for relational systems like Oracle, partitioning and indexing for query performance. For OpenSearch, time-based indices and proper shard count for parallel execution.
>
> For heavy workloads, asynchronous processing via Kafka or SQS so the system absorbs spikes without impacting user-facing latency.
>
> Trade-offs: stateless services scale easily at the application layer, but the data layer still has consistency-vs-availability trade-offs. Asynchronous processing improves scalability but introduces eventual consistency. More partitions improve performance but add operational complexity."

---

### Q6: Multi-layer scaling approach

**Memory Hook:** Edge → API → Compute → Data → Async

```
Edge       → CDN for static, caching at edge
API        → API gateway with routing, throttling, security
Compute    → Kubernetes HPA, auto-scaling cloud services
Data       → Partitioning, indexing, sharding for horizontal scale
Async      → Kafka/SQS for decoupling and spike absorption
```

---

### Q7: Scale from 120 to 1200 customers

**Memory Hook:** Diagnose → Quick Wins → Horizontal → Data → Ops → Team → Incremental

> "Scaling 10x is primarily organizational and operational, not just technological. Cloud handles compute elasticity — that is the easy part."

**Critical lesson from Ananth at Availity:** he redirected this question 3 times when the answer kept going back to infrastructure. **Lead with team and quality. Infrastructure last.**

> **Step 1 — Diagnose.** Profile the actual bottleneck. DB? A specific service? Network? Scale the constraint, not everything.
>
> **Step 2 — Quick wins before re-architecture.** Redis caching for read-heavy data. Connection pool tuning (HikariCP). Query optimization with execution plans and composite indexes. CDN for static assets.
>
> **Step 3 — Horizontal scaling.** Stateless services + Kubernetes HPA. Load balancer tuning. AKS/EKS node pool scaling.
>
> **Step 4 — Data layer scaling.** Read replicas. Tenant-based partitioning in Oracle. Index-per-tenant or time-based in OpenSearch. S3 lifecycle policies for cold data.
>
> **Step 5 — Operational readiness.** SLOs defined per service before next customer cohort. Alerting on p99 latency, error rate, Kafka consumer lag. Runbooks for every tier-1 failure mode.
>
> **Step 6 — Team scaling.** Domain-based ownership. Platform team for shared infrastructure. 30–40% headcount growth for 10x customer growth (not 1:1 — established teams are more efficient).
>
> **Step 7 — Incremental rollout.** Scale one layer at a time. Load test before each major customer onboarding. Canary deployments for risky changes.

**Real example (Cerner):**
> "When Care Management grew from 120 customers, OpenSearch latency climbed from 80ms to 700ms on readmission risk queries. Root cause: hot shards because all tenants shared one index. We introduced tenant-based index routing and time-based rollover — latency back to 95ms. For DB layer, connection pool exhaustion was next — we tuned HikariCP and added Redis cache for reference data, cutting Oracle load by 40%. AKS HPA on API pods scaled them automatically during peak alert windows. Each step was load-tested before the next customer cohort onboarded."

---

### Q8: Design a high-throughput system

**Memory Hook:** Edge → App → Data → Resilience

> "Remove bottlenecks across all layers.
>
> **Edge.** CDN and API gateway for traffic distribution, caching, rate limiting.
>
> **Application.** Stateless services scale horizontally. Async processing for heavy workloads — user requests not blocked.
>
> **Data.** Critical for throughput. Oracle: partitioning, indexing, query optimization. OpenSearch: time-based partitioning, appropriate shard counts for parallel execution.
>
> **Resilience.** Circuit breakers, retries with backoff, observability to detect and recover from failures.
>
> Trade-offs: aggressive caching improves throughput but introduces stale data. Heavy sharding improves performance but increases operational overhead.
>
> Real example: improved throughput by introducing Redis caching, moving heavy workflows to Kafka async, and optimizing OpenSearch shard configuration. Significantly improved capacity and reduced response times under load."

---

## SECTION C — DISTRIBUTED SYSTEM PATTERNS

---

### Q9: Backpressure handling

**Memory Hook:** Rate Limit → Circuit Breaker → Queue Buffer → Reactive → Bulkhead

**Simple analogy:** Water tank fills at 100L/min, pipe to bucket emptying at 20L/min. Without flow control, bucket overflows. Backpressure tells the tank to slow down.

#### 5 strategies:

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
    .flatMap(event -> process(event), 16)  // max 16 concurrent
    .subscribe();
```

**5. Bulkhead — isolate failures to a pool**
```java
Bulkhead.of("opensearch", BulkheadConfig.custom()
    .maxConcurrentCalls(20).build());
// OpenSearch slow → 20 threads affected → other services unaffected
```

#### Production layering:
```
Request → Bulkhead → TimeLimiter → CircuitBreaker → Retry → call → Fallback
```

---

### Q10: Data consistency in distributed systems

**Memory Hook:** Correctness → Strong | Scale → Eventual | Patterns → Saga/Outbox

> "Apply consistency based on operation criticality, not uniformly.
>
> Financial transactions or critical clinical decisions — strong consistency. Correctness cannot be compromised.
>
> Analytics, notifications, reporting — eventual consistency. Slightly stale data is acceptable if throughput and availability are higher.
>
> Cross-service workflows — Saga pattern. Reliable event publishing — Outbox pattern. Idempotency across all async operations.
>
> CAP theorem is real: in distributed systems during network partition you choose between consistency and availability. Make this choice explicitly per use case, not by accident."

---

### Q11: SAGA Pattern — Choreography vs Orchestration

**Memory Hook:** Distributed Tx → Saga | Choreo → Simple | Orchestrator → Control

> "I use Saga when managing transactions across multiple services in a distributed system, where traditional ACID transactions are not feasible.
>
> Workflow is broken into a series of local transactions. If any step fails, compensating actions trigger to maintain consistency.
>
> **Choreography** — services communicate through events and react independently. Loose coupling. Works well for simple flows. Hard to track and debug as the system grows.
>
> **Orchestration** — central orchestrator controls the sequence and handles failures explicitly. Better visibility and control. Tighter coupling.
>
> My preference for critical workflows: orchestration. Better debugging. Explicit error handling. Worth the central dependency.
>
> Real example at Cerner: simple workflows like notifications used choreography. Critical workflows like patient processing → risk evaluation → notification used an orchestrator for explicit control."

#### Compensating Actions example
```java
// Step 1: Reserve inventory  →  SUCCESS: proceed
// Step 2: Create pick task   →  FAIL: COMPENSATE Step 1 (release reservation)
// Step 3: Generate shipment  →  FAIL: COMPENSATE Steps 1 & 2

void releaseInventoryReservation(String reservationId) {
    inventory.updateStatus(reservationId, RELEASED);
    outboxRepo.save(new OutboxEvent("INVENTORY_RELEASED", reservationId));
}
```

---

### Q12: Outbox Pattern — Why SAGA Alone Is Not Enough

**Memory Hook:** DB ✅ Event ❌ → Outbox

> "Common consistency problem: DB transaction succeeds but event publication to Kafka fails. Mismatch between system state and downstream consumers.
>
> Without Outbox:
> ```java
> @Transactional
> void reserveInventory() {
>     repo.save(reservation);                     // committed
>     kafkaTemplate.send("events", payload);      // CRASH HERE
>     // DB committed, event never sent. Ghost state.
> }
> ```
>
> With Outbox: write the event to the DB in the same transaction as the state change.
>
> ```java
> @Transactional
> void reserveInventory(String sku, int qty) {
>     repo.save(new Reservation(sku, qty, RESERVED));
>     outboxRepo.save(OutboxEvent.builder()
>         .eventType("INVENTORY_RESERVED")
>         .status(PENDING).build());
>     // Crash here → both writes roll back → clean state
> }
>
> // Separate poller publishes PENDING events
> @Scheduled(fixedDelay = 100)
> void publishPendingEvents() {
>     outboxRepo.findByStatus(PENDING).forEach(event -> {
>         kafkaTemplate.send("events", event.getPayload());
>         outboxRepo.updateStatus(event.getId(), PUBLISHED);
>     });
> }
> ```
>
> Trade-offs: improves reliability and consistency but adds complexity managing the outbox table and background processing. Slight latency in event propagation since events publish asynchronously.
>
> To make it more robust, combine with: idempotent consumers, retry with exponential backoff, DLQ for persistent failures, CDC tools like Debezium for streaming outbox events."

---

### Q13: Payment service failure — design response

**Memory Hook:** Fail Fast → Retry → Protect → Compensate

> "When payment fails, my priority is system stability while maintaining good user experience.
>
> **Fail fast** — do not block the system. Prevents cascading failures.
>
> **Retry with backoff** for transient issues. Exponential backoff for transient failures.
>
> **Circuit breaker** to stop repeated calls to a failing service and avoid overwhelming it.
>
> **User feedback** — clear feedback. Allow user to retry or show meaningful failure state. Never leave system in uncertain state.
>
> **Saga compensation** — for distributed workflows. If payment fails after order creation, trigger compensating actions (cancel order, release inventory).
>
> Trade-offs: retries improve success but increase latency. Circuit breakers protect but may temporarily reject valid requests. Compensation ensures consistency but adds workflow complexity.
>
> To strengthen: idempotency keys to prevent duplicate payments, DLQ for failed events, monitoring for failure rates, graceful degradation when providers are unavailable."

---

### Q14: How do you design for failure?

**Memory Hook:** Assume → Isolate → Recover → Degrade

> "I design assuming failures will happen, especially in distributed environments.
>
> **Isolation** — failures in one component don't cascade. Service boundaries, bulkheads, circuit breakers.
>
> **Recovery** — for transient failures, retries with exponential backoff. For persistent failures, fallback so the system continues operating in limited capacity.
>
> **Graceful degradation** — instead of failing completely, provide partial functionality. Serve cached data. Disable non-critical features.
>
> **Observability** — strong monitoring with metrics, logs, tracing. Alerts so failures are detected early.
>
> Trade-offs: resilience adds complexity. Overusing retries can amplify load during failures.
>
> Real example: downstream service failures were impacting user-facing APIs. Introduced circuit breakers and fallback responses, plus retries for transient issues. System remained stable even when dependencies were partially unavailable."

---

### Q15: How do you design for high availability?

**Memory Hook:** No SPOF (Single Points of Failure) → Redundancy → Failover → Multi-Region

> "Ensure no single point of failure across any layer.
>
> **Infrastructure.** Deploy across multiple availability zones, load balance across them. If one AZ fails, traffic routes to healthy zones automatically.
>
> **Application.** Stateless services + Kubernetes health checks + auto-healing. Unhealthy pods replaced without manual intervention.
>
> **Data.** Replication — read replicas in Oracle, replica shards in OpenSearch. Data available even if primary fails.
>
> **Failover.** Automatic within region. For critical systems, active-active multi-region with global load balancing.
>
> Trade-offs: multi-region adds cost, cross-region consistency complexity, latency. Justify based on RTO/RPO requirements — not every system needs multi-region.
>
> For critical systems, also consider: active-active vs active-passive multi-region, RTO/RPO planning, DNS-based or global load balancing for failover, regular failover testing."

---

## SECTION D — SPECIFIC SYSTEM DESIGNS

---

### Q15a: Active-Active design — what happens when the local DB goes down mid-transaction?

**Memory Hook:** Atomicity at Local Zone → User Failure → Retry → Other Zone → Eventual Catch-up

This came up at TJX. You took 3 turns to land it. Memorize this clean answer.

> "If the local DB write and the outbox table write don't both succeed, the @Transactional rolls back — atomicity is preserved at the local zone level. The user sees a failed request.
>
> On retry, traffic may route to the healthy zone where the transaction completes. Worst case is brief unavailability for the affected users — not data inconsistency.
>
> Eventual consistency between zones is handled by Kafka-based replication of outbox events. Once the failed zone recovers, it catches up via the same Kafka topic.
>
> Two key design points:
> - Atomicity is local — guaranteed by the transactional boundary in the failing zone
> - Cross-zone consistency is eventual — guaranteed by the outbox + Kafka pattern
>
> What I would NEVER do: try to write across zones synchronously to mask the failure. That breaks the active-active model and creates worse failure modes than the one we're trying to handle."

#### When TJX-style mediation layer changes this

If the application is a **stateful mediation layer** (like the TJX Manhattan WMS context) where in-flight transactions must finish in the same zone they started, the design changes:
- Sticky session routing at the application layer (not just geolocation)
- Transaction affinity — once a transaction starts in zone A, all subsequent calls go to zone A until commit
- DR fallback only for new transactions, not for in-flight ones

> "For a mediation layer with stateful in-flight transactions, geolocation routing is the wrong abstraction. You want session/transaction affinity at the application layer. The active-active design protects against zone failure for new traffic — not for transactions already in progress."

---

### Q16: Design a Notification System

**Memory Hook:** Event → Queue → Processor → Channel → Retry → Status

```
Business Event (order placed, patient alert)
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

#### Critical: How do you respond back with success/failure status?

**Pattern 1 — Async callback (preferred):**
> "When a producer submits a notification request, they include a correlation ID and a reply-to Kafka topic. When our notification service finishes — success or failure — we publish the result to that reply-to topic with the same correlation ID. The producer subscribes and matches."

**Pattern 2 — Polling endpoint (for sync consumers):**
> "GET /notifications/{correlationId}/status returns PENDING, DELIVERED, FAILED, or DEAD_LETTERED. State stored in a status table that the notification service updates on every transition."

**Pattern 3 — Permanent failures:**
> "After retries exhaust, the message lands in DLQ with error code and reason. Operations alerted. We expose a replay endpoint for manual intervention after fixing root cause."

**Key design decisions:**
- At-least-once delivery with idempotent processing — not exactly-once (too complex, unnecessary)
- Rate limiting per channel provider — email vendors have rate caps
- User preferences: opt-in/opt-out stored in preferences service
- DLQ for permanent failures — ops reviews daily, replay after fix

---

### Q17: How do you scale notification service from 1M to 5M orders/day?

**Memory Hook:** Pod Scale + Partition Scale + Provider Limits

> "Two layers — processing and messaging.
>
> **Processing.** Kubernetes HPA on CPU at 70% (not 80% — scaling takes time, you want headroom). Min and max pod counts based on observed traffic. For seasonal spikes, pre-scale before the event rather than reacting.
>
> **Messaging.** Scale Kafka consumer group partitions. Each consumer reads from one partition. 1M to 5M means 5x consumer throughput — increase partition count accordingly. Kafka handles buffering automatically.
>
> Key parameters: consumer lag in Kafka, pod CPU saturation, **email provider API rate limits** — the external vendor is often the actual bottleneck at 5M/day, not our own service.
>
> Cost: spot instances or preemptible VMs for burst capacity — stateless consumer, failures restart from last committed Kafka offset."

---

## SECTION E — DATA STORE DESIGN (PARTITIONING & SHARDING)

---

### Q18: Partitioning vs Sharding — clean definitions

**This is the question Cubic asked. The wrong answer cost you grade. Memorize the correct definitions.**

#### Partitioning
**Splitting data within ONE database instance** by a logical key — for example, partitioning a patient table by tenant_id so each tenant's data is in a separate partition. Reads for one tenant do not touch another tenant's partition.

**PostgreSQL example:**
```sql
CREATE TABLE orders (
    order_id   SERIAL,
    order_date DATE NOT NULL,
    amount     NUMERIC
) PARTITION BY RANGE (order_date);

CREATE TABLE orders_2024 PARTITION OF orders
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
```

Three strategies: RANGE (dates, IDs), LIST (country, status), HASH (even distribution).

#### Sharding
**Distributing partitions ACROSS MULTIPLE physical database instances or nodes.** Each shard is a separate DB server holding a subset of total data. Horizontal scaling at the data layer — no single instance is the bottleneck. Cross-shard queries become complex; you need a routing layer.

#### Clean rule of thumb (memorize)
- **Partitioning** = splitting data on **one machine**
- **Sharding** = splitting data across **many machines**
- **Sharding is partitioning at distributed scale** — superset

#### OpenSearch — sharding by default
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

Shard count is fixed at index creation — hard to change without reindexing. Replicas add fault tolerance, not additional primary storage.

#### RDBMS sharding options
- Citus extension for PostgreSQL — distributes tables across nodes
- Vitess — sharding middleware for MySQL (used by YouTube, GitHub)
- Application-level sharding — app decides which DB server based on key

#### OpenSearch partitioning options
- Index-per-tenant — one index per customer
- Time-based indices — orders-2024, orders-2025 (prune by index, not by scan)
- Custom routing keys — force related documents to same shard

---

### Q19: Database access patterns

> "Depends on the datastore.
>
> **OpenSearch** — index design, correct field mapping, query optimization (filters, avoiding deep pagination), sharding strategy.
>
> **Oracle** — indexing strategy, partitioning, query optimization via execution plans, reducing DB calls through caching.
>
> Real example at Cerner: queries slowed due to large table scans on a patient processing system. Fix: added composite indexes, introduced date-based partitioning. Query time reduced from seconds to milliseconds."

---

## SECTION F — JAVA / SPRING BOOT MICROSERVICES (JPMC STACK)

The JD calls out Java/Spring Boot, REST APIs, and microservices specifically. Be ready to talk about them at design and operations level — not just syntax.

---

### Q20: How do you design REST APIs and Spring Boot microservices for production?

**Memory Hook:** Contract → Boundary → Resilience → Observability → Secure

> "Five disciplines I enforce on every Spring Boot service.
## **Memory Hook:** Contract → Boundary → Resilience → Observability → Secure

> **API Governance - Contract first**  
> I always start with a contract-first approach. Every API is defined using OpenAPI and goes through design review before any code is written.  
>
> We treat APIs as products, not implementation details.  
> Versioning is explicit—/v1, /v2—and we follow a strict no-breaking-changes policy. If a change is unavoidable, we run a formal deprecation cycle and support N-1 versions.  
>
> This prevents downstream failures and gives consumer teams confidence to integrate.

> **Domain Integrity - Clear service boundaries**  
> I align services with bounded contexts to ensure high cohesion and low coupling.  
>
> For example, in an account origination domain, I would separate services like application-intake, KYC, risk scoring, and decisioning.  
>
> Each service owns its own database, and I enforce a hard rule: no cross-service database joins. If we need shared data, we use APIs or asynchronous events.  
>
> This ensures scalability and prevents tight coupling across teams.

> **Engineering for Failure - Resilience**  
> Infrastructure will fail, so the application must be defensive.  
>
> I leverage Resilience4j to implement circuit breakers, bulkheads, and retries with exponential backoff.  
>
> The goal is to prevent a single downstream delay from cascading into a total system blackout—an anti-fragile approach I've used to mitigate high-pressure incidents in the past.  
>
> This builds systems that degrade gracefully instead of failing catastrophically.

> **Observability - No Dark Corners Rule**  
> A service is invisible without telemetry.  
>
> I bake in Spring Boot Actuator and Micrometer from day one.  
>
> We use structured JSON logging and propagate correlation IDs through every hop to enable distributed tracing.  
>
> My policy is simple: no dashboard, no deploy.  
>
> We need to see the health of the system before users report issues.

> **Security - Secure by Default**  
> Security isn't a perimeter; it’s a layer.  
>
> We validate OAuth2/JWT at the API Gateway and re-verify at the service level (Zero Trust).  
>
> Beyond that, I enforce strict Bean Validation on all inputs to prevent injection and ensure secrets are never in properties files, but pulled from a secure vault at runtime.  
>
> We also enforce audit logging for sensitive operations.  
>
> This ensures systems are secure, compliant, and production-ready by default.

---

### Q21: How do you decide REST vs Kafka for inter-service communication?

**Memory Hook:** Sync for Read → Async for Write → Saga for Distributed Workflow

> "Three rules.
>
> **REST for queries.** When a service needs immediate data — fetch customer profile during origination, validate eligibility — synchronous REST is fine. Latency cost is acceptable, and the consumer needs the answer to proceed.
>
> **Kafka for state changes that fan out.** When an event triggers downstream work — application.submitted triggers KYC, document verification, risk scoring, notifications — async events decouple producers from consumers. Producers don't wait. Consumers can scale independently.
>
> **Saga for distributed workflows.** Account origination is a multi-step workflow across services. We use either choreography — services react to each other's events — or orchestration with a coordinator service when sequencing is critical. Add Outbox pattern to guarantee no event is lost when a service crashes mid-transaction.
>
> Default: REST for reads, Kafka for writes that fan out, Saga + Outbox for true distributed transactions."

---

### Q22: How do you ensure Spring Boot service performance and tune it?

**Memory Hook:** Measure → Hot Path → Cache → JVM → Async

> "Five-step approach.
>
> **Measure first.** APM — New Relic, Dynatrace — gives p50/p95/p99 latency, throughput, GC behavior. No optimization without data.
>
> **Optimize the hot path.** Database queries are the most common killer — N+1 patterns, missing indexes, full-table scans. Reduce serialization where possible. Lazy-load only what's needed.
>
> **Cache deliberately.** Redis for shared cache, Caffeine for in-process hot data. Always with TTL and clear invalidation. Cache without eviction is a memory leak waiting to happen — I caught one at Cerner that improved memory by 40%.
>
> **Tune JVM.** Right heap size based on observed allocation, G1GC default for most workloads, Tomcat thread pools sized to actual concurrency. Don't tune blind — change one variable, measure, repeat.
>
> **Async where it fits.** @Async, CompletableFuture, or WebFlux for I/O-heavy paths. Don't make everything reactive — it's only worth it when I/O concurrency is the actual bottleneck."

---

### Q23: How do you secure REST APIs in a banking context?

**Memory Hook:** Auth → AuthZ → Validate → Encrypt → Audit

> "Five layers.
>
> **Authentication** — OAuth2/OIDC with JWT, validated at the API gateway and re-validated at each service for defense-in-depth. No anonymous access on internal APIs.
>
> **Authorization** — RBAC and attribute-based controls via Spring Security. Method-level annotations for fine-grained checks. Principle of least privilege.
>
> **Input validation** — Bean Validation at the controller. Reject bad input at the edge, never trust downstream. SQL injection, XSS, schema violations — all caught here.
>
> **Encryption** — TLS in transit, AES-256 at rest, secrets in HashiCorp Vault or equivalent. Never in config files, never in environment variables for sensitive secrets.
>
> **Audit logging** — Every privileged action — view, modify, approve — logged with who, what, when, correlation ID. Required for SOX, PCI, and regulator inquiries."

---

### Q24: How do you handle errors and exceptions consistently across services?

**Memory Hook:** Centralize → Standardize → No Leaks

> "Three rules.
>
> **Centralize.** @ControllerAdvice with @ExceptionHandler for global handling. No try-catch noise scattered through controllers.
>
> **Standardize the response format.** RFC 7807 Problem Details — type, title, status, detail, instance. Clients parse one format across all services. Internally generated correlation ID echoed back so support can trace.
>
> **No information leakage.** Stack traces, DB schema, internal paths — all stay in logs. Clients see safe, generic messages. Map domain errors properly: ValidationException → 400, AuthorizationException → 403, NotFound → 404. Don't return 500 for everything."

---

## SECTION G — KAFKA & EVENT-DRIVEN ARCHITECTURE (JPMC STACK)

The JD calls out Kafka producers, consumers, and streaming pipelines for real-time data processing. Be ready for both design and operational depth.

---

### Q25: How do you design Kafka topic, partitions, and retention?

**Memory Hook:** Partitions for Scale → Replication for Durability → Retention for Use Case

> "Four decisions for every topic.
>
> **Partitions** — set based on expected peak throughput and consumer parallelism. More partitions = more parallel consumers, but rebalancing overhead grows too. Start with what you need; over-partitioning is hard to undo.
>
> **Partition key** — chosen carefully. For account events, account-id gives ordering per account but can hot-spot if traffic is skewed. For events where global ordering doesn't matter, hash distribution is fine.
>
> **Replication factor** — 3 in production with min.insync.replicas=2. Tolerates one broker failure without data loss. Non-negotiable for tier-1 topics.
>
> **Retention** — operational topics 3–7 days. Audit and replay topics 30+ days, or compacted indefinitely. For latest-state topics — like a customer profile snapshot — log compaction keeps the latest value per key forever."

---

### Q26: How do you ensure exactly-once or at-least-once delivery?

**Memory Hook:** Idempotent Producer → Transaction → Idempotent Consumer

> "Three layers.
>
> **Idempotent producer.** enable.idempotence=true prevents duplicate writes from retries within Kafka itself.
>
> **Transactions for atomic multi-topic writes.** When a single business event needs to land in two topics atomically — say, account.created and audit.event — wrap in a Kafka transaction. Same for read-process-write patterns.
>
> **Idempotent consumer is the realistic guarantee.** True end-to-end exactly-once across producer, broker, consumer, and downstream sink is rare in practice. Instead, design consumers to be idempotent — use a deduplication key (event ID) and check before processing. UPSERT, not blind INSERT. Operations safe to repeat.
>
> The Outbox pattern (covered in Section C) closes the gap when a producer crashes between DB commit and Kafka send."

---

### Q27: How do you handle errors and dead letter queues in Kafka consumers?

**Memory Hook:** Categorize → Retry → DLQ → Replay → Alert

> "Five-step pipeline.
>
> **Categorize errors first.** Transient — network, downstream timeout — vs permanent — bad payload, schema violation. Different errors need different handling.
>
> **Retry transient errors with backoff.** Local retry first (one or two attempts), then a dedicated retry topic with delay if it persists.
>
> **DLQ for permanent failures.** Send the original payload, error, timestamp, and headers to a dead-letter topic. Don't lose data; quarantine it.
>
> **Replay capability.** Tooling to push DLQ messages back into the main topic after the fix is deployed. DLQ is a triage queue, not a graveyard.
>
> **Alert on DLQ growth.** Threshold-based alert. If DLQ is filling up, something is broken upstream."

---

### Q28: How do you monitor Kafka pipelines in production?

**Memory Hook:** Lag → Throughput → Errors → Health → Alert on SLO

> "Five signals.
>
> **Consumer lag.** The single most important metric. Lag growth means consumers can't keep up. Alert when lag exceeds your data-freshness SLO.
>
> **Throughput.** Messages per second per topic, per consumer group. Sudden drops are leading indicators of problems.
>
> **Error rate.** Failed messages, deserialization errors, retries, DLQ writes. Trend matters more than absolute number.
>
> **Cluster health.** Under-replicated partitions, ISR shrinks, broker disk usage. Platform-team responsibility, but I monitor as a consumer of the platform.
>
> **Alerts on SLO breach, not infra noise.** Lag > X minutes, error rate > Y%, throughput drop > Z%. Pages should be actionable."

---

### Q29: How do you handle Kafka schema evolution?

**Memory Hook:** Registry → Compatibility → Additive → Version Explicit

> "Four practices.
>
> **Schema Registry.** Confluent or equivalent. All schemas versioned, validated, and central. Producers and consumers register what they expect.
>
> **Compatibility rules.** BACKWARD by default — new consumers can read old data. FULL for shared, critical topics — old and new consumers coexist.
>
> **Additive changes only.** Add optional fields with defaults. Never remove or rename existing ones. Most so-called 'breaking changes' can be done as additive.
>
> **Version explicitly when truly breaking.** Create account.created.v2. Run both topics in parallel during deprecation. Monitor v1 consumer count. Retire only when truly unused — not when you wish it was unused."

---

### Q30: Design a Kafka-driven event flow for an account origination journey

**Memory Hook:** Capture → Orchestrate → Aggregate → Notify → Audit

> "Five stages, all event-driven.
>
> **Capture.** application.submitted is published when the customer completes the form. Single source of truth for downstream.
>
> **Orchestrate downstream work.** KYC service consumes and runs identity check. Document service consumes and processes uploads. Risk service consumes and runs scoring. Each emits its own completion event — kyc.completed, documents.verified, risk.cleared.
>
> **Aggregate state.** An orchestration service consumes the per-step events and emits application.approved or application.rejected based on rules. This is where Saga choreography lives.
>
> **Notify and update UI.** Notification service consumes the final state event and sends email/SMS. UI subscribes via WebSocket or polls a status endpoint backed by the consolidated event.
>
> **Audit trail.** All events retained in a compacted audit topic for compliance, replay, and analytics. Required for regulator inquiries — bank's reality.
>
> Three reliability anchors: Outbox pattern for guaranteed event publication, idempotent consumers for safe retries, DLQ + replay for poison messages."

---



| Pattern | One-line | Use when |
|---------|----------|----------|
| SAGA | Distributed transaction with compensating ops | Multi-service workflow without single DB transaction |
| Outbox | Event written in same DB tx as state change | Guarantee Kafka publish even on crash |
| Circuit Breaker | Stop calling a failing service | Dependency that fails consistently |
| Bulkhead | Isolate thread pools per downstream | One slow service starving others |
| Idempotency | Same op multiple times = same result | Any retry-able operation |
| CQRS | Separate read/write models | High R:W ratio with different access patterns |
| Event Sourcing | State = sequence of events | Audit / time-travel queries needed |
| Blue-Green | Two live envs, cut traffic over | Major releases needing instant rollback |
| Canary | Deploy to small % first | Gradual rollout with real validation |
| Partitioning | Split data within one DB instance | Query performance on large tables |
| Sharding | Split data across multiple DB instances | Horizontal scale beyond single instance |
| Reactive Streams | Demand-based flow control | Backpressure in async pipelines |

---

## QUICK REFERENCE — CAP, ACID, BASE

**CAP Theorem:** Distributed system can guarantee at most 2 of 3:
- Consistency — all nodes see same data
- Availability — system always responds
- Partition tolerance — system survives network partitions

**Real-world choice:** P is non-negotiable in distributed systems. So you choose between C and A.
- CP systems: HBase, MongoDB (configurable), traditional RDBMS clusters
- AP systems: Cassandra, DynamoDB

**ACID** (relational): Atomicity, Consistency, Isolation, Durability
**BASE** (NoSQL/distributed): Basically Available, Soft state, Eventual consistency

---

*File 4 of 8 — System Design, Architecture, Java/Spring Boot & Kafka*
