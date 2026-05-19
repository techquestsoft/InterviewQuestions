# FILE 4 OF 8 — SYSTEM DESIGN, ARCHITECTURE, JAVA / SPRING BOOT & KAFKA

> **Rule 1:** Never start with architecture. Always: **Goals → Metrics → Constraints → NFRs → Levers → Options → Architecture.**
> **Rule 2:** Every design decision needs a trade-off. **No trade-off = not senior-level thinking.**
> **Rule 3:** Use real numbers — 5M weekly transactions, 110+ services, 500M patient records.

**Structure of every answer:** Memory Hook → Core Answer (framework + reasoning) → Example (specific story with numbers, code, or trade-off).

---

## CROSS-FILE INDEX

This file owns: design framework, distributed system patterns, monolith vs microservices, scaling, backpressure, HA, notification system, partitioning/sharding, Java/Spring Boot service patterns, Kafka producer/consumer/streaming patterns.

- Production reliability, observability, cloud, Kubernetes, databases → File 06
- Data quality, ETL, data integrity → File 08
- GenAI architecture → File 07
- UI architecture (React/Angular) → File 05

---

## THE UNIVERSAL SYSTEM DESIGN OPENER

> *"Before I jump into the design, I want to spend two to three minutes clarifying goals, success metrics, constraints, and NFRs — because every architecture decision flows from these. Is that OK?"*

**This single signal separates senior from junior.** Junior candidates draw boxes immediately.

---

## DESIGN FRAMEWORK — APPLY TO EVERY QUESTION

```
Step 1: GOAL         — What problem? Who are the users?
Step 2: METRICS      — How do we measure success? (latency, throughput, accuracy)
Step 3: CONSTRAINTS  — What cannot change? (existing systems, budget, compliance)
Step 4: NFRs         — Scalability, reliability, availability, security, idempotency
Step 5: LEVERS       — Break system into 4–5 control points
Step 6: OPTIONS      — A (simple), B (recommended), C (complex). Pick B with reasoning.
Step 7: ARCHITECTURE — Components, data flow
Step 8: FAILURES     — What breaks? Mitigation?
Step 9: TRADE-OFFS   — What was sacrificed? Alternatives?
Step 10: PHASES      — Build incrementally
```

---

# SECTION A — CORE DESIGN PHILOSOPHY

---

## Q1: How do you approach system design?

**Memory Hook:** Clarify Problem → Break Into Layers → Evaluate Options → Evaluate Trade-offs → Align With Enterprise Standards

> **Core Answer**
>
> I approach system design **business-first, not technology-first**.
>
> First, I clarify the problem — what we're building, who the users are, what scale we're targeting. Along with that, I identify key NFRs like latency, availability, and throughput — these directly drive architectural decisions.
>
> Once the problem is clear, I break the system into logical layers — typically APIs, processing, and data — to identify responsibilities and bottlenecks.
>
> Then I evaluate architectural options. Whether a modular monolith is sufficient or we need microservices or event-driven design — based on domain boundaries, scaling needs, and team structure.
>
> Critically, I evaluate **trade-offs**: scalability, maintainability, cost, and operational complexity. **I do not over-engineer early.**
>
> Finally, I ensure the design aligns with enterprise standards — security, observability, governance — so it's production-ready and compliant from day one.

---

## Q2: Monolith vs Microservices

**Memory Hook:** Team Structure + Domain Complexity + Scaling Granularity → Architecture Choice

> **Core Answer**
>
> The decision depends on three factors: **team structure, domain complexity, and scaling granularity needed.** I do not default to microservices.
>
> **Monolith** is right when the domain is unified, the team is small, change frequency is low, scale is modest. My BoA campaign tool served 1,000–2,000 internal users. Microservices would have added overhead with zero benefit.
>
> **Microservices** are right when different parts have genuinely different scaling needs and change rates. At Optum, the eligibility API and claims API had completely different load patterns. A monolith forces you to scale everything together.
>
> On *"can we scale a monolith behind a gateway?"* — yes, you can scale horizontally. The problem is **granularity**. If eligibility is the bottleneck, you scale the entire monolith, paying for compute claims and benefits do not need. Kubernetes gives pod-level precision.
>
> **My preference:** start modular monolith, extract to microservices when domain boundaries are clear, team structure supports it, and observability is in place. **Premature microservices is a common enterprise anti-pattern.**
>
> **Trade-off Table**
>
> | | Monolith | Microservices |
> |--|---------|--------------|
> | Build speed | Fast initially | Slower — more infrastructure |
> | Debugging | Easy — single codebase | Hard — distributed tracing required |
> | Scaling | Coarse-grained (single API: `GET /patient-dashboard`) | Fine-grained: `GET /patient`, `GET /risk-score`, `GET /medications`, `GET /insurance` |
> | Team independence | Low | High |
> | Operational overhead | Low | High |
> | Network latency | None | Real cost |

---

## Q3: What is the Strangler Pattern?

**Memory Hook:** Gradual Replacement → Reduce Operational Risk → Continuous Business Delivery

> **Core Answer**
>
> The **Strangler Pattern** is a modernization strategy where legacy functionality is gradually replaced piece by piece rather than rewriting the entire system at once.
>
> A facade or routing layer sits in front of the legacy system. New functionality goes to the new system; old functionality continues on the legacy system. Over time, more functionality is "strangled" away from the legacy system until it can be retired. **Reduces operational risk while allowing continuous business delivery during modernization.**
>
> **Example**
>
> In healthcare systems, modernization typically starts with **supporting services first — notifications, APIs, reporting** — before core transactional workflows are decomposed. This protects clinical workflows from migration risk and lets the team build modernization muscle on lower-stakes services. Big-bang rewrites are almost always wrong for regulated, production-critical platforms.

---

## Q4: 12-Factor App Principles

**Memory Hook:** Stateless Processes → Config Via Environment → Logs As Streams → Backing Services Attached → Dev/Prod Parity

> **Core Answer**
>
> | Factor | Meaning | Why |
> |--------|---------|-----|
> | **Stateless processes** | No session state in app layer | Horizontal scaling — any pod serves any request |
> | **Config via environment** | No hardcoded values | Same codebase across dev / staging / prod |
> | **Logs as streams** | Write to stdout | Centralized to Splunk via FluentBit |
> | **Backing services attached** | DB, cache, queue external | Easy to swap, easy to test |
> | **Dev/prod parity** | Same environments | No "works on my machine" |
>
> **Example**
>
> All Cerner microservices follow 12-factor — config via Kubernetes secrets, logs streamed to Splunk via FluentBit, stateless pods that auto-scale with HPA. This is what makes cloud-native scaling and zero-downtime deployment possible in the first place.

---

## Q5: What changes beyond architecture when systems scale?

**Memory Hook:** Technology → Operational Maturity → Governance → Process and Team Structure

> **Core Answer**
>
> When systems scale, more than architecture needs to evolve.
>
> **Technology** — architecture handles higher load, but engineering quality matters equally. Stricter coding standards, automated testing, CI/CD quality gates.
>
> **Operational maturity** becomes critical. Observability (metrics, logs, tracing) plus clearly defined SLOs and alerting so issues are detected and resolved fast.
>
> **Governance** — release controls, approval workflows, audit trails, compliance checks. Especially in regulated environments like healthcare.
>
> **Process and team structure** — domain ownership, standardized practices across teams to avoid fragmentation.
>
> **Example**
>
> At Cerner, as we scaled, we introduced **CI/CD quality gates with security scans, defined SLOs with centralized observability, added CAB approvals and audit trails** for production releases. System reliability and compliance improved without crushing delivery velocity.

---

## Q6: Architecture Governance vs Architecture Review

**Memory Hook:** Review = Point-in-Time → Governance = Continuous

> **Core Answer**
>
> **Architecture review** is a point-in-time validation of a specific solution design — focusing on scalability, resiliency, security, and operational readiness. It typically happens before implementation begins.
>
> **Architecture governance** is broader and continuous — it ensures teams consistently follow enterprise standards, security controls, observability practices, and strategic technology direction over time. Examples: API standards, resiliency patterns, telemetry standards, security baselines.
>
> **Example**
>
> An architecture review might validate whether a specific microservice design is appropriate for a use case. Architecture governance ensures **all engineering teams across the organization follow common API, resiliency, and observability standards** — preventing the kind of drift where each team builds its own auth, its own retry logic, and its own logging schema. At Oracle Cerner, OHRM and the HDI architecture forums are governance mechanisms; individual project design reviews are reviews.

---

## Q7: How do you modernize legacy monolithic systems?

**Memory Hook:** Assess → Decompose Strategically → Incremental Cutover → Govern the Transition

> **Core Answer**
>
> My approach to legacy modernization is **incremental, not big-bang**. I first identify operational bottlenecks and bounded contexts, then gradually modernize through APIs, event-driven integration, and phased decomposition — while maintaining operational stability.
>
> Four steps:
>
> **Assess** — profile incident sources, change-frequency hotspots, and bounded contexts within the monolith. Decompose where natural seams exist, not where it looks clean on paper.
>
> **Decompose strategically** — start with high-change or operationally expensive areas (reporting, notifications, integrations) before touching core transactional domains.
>
> **Incremental cutover** — use the Strangler Pattern (Q3). New functionality goes to new services. Old continues on the monolith until ready to retire.
>
> **Govern the transition** — architecture standards, API contracts, observability, and security posture must be consistent across old and new. Otherwise modernization creates a "distributed monolith" — worse than the original.
>
> **Example**
>
> In healthcare platforms, modernization typically starts with **reporting, notifications, and integrations** before addressing core transactional domains like patient processing or risk scoring. The reasons: lower regulatory risk, faster ROI, and a real proof-of-pattern before touching clinical workflows.

---

# SECTION B — SCALING

---

## Q8: How do you design for scalability?

**Memory Hook:** Stateless Services → Distribute Traffic → Optimize Data Layer → Asynchronous Processing

> **Core Answer**
>
> **Stateless services first** — replicable and scalable via Kubernetes. Then **distribute traffic** via API gateway and load balancers.
>
> **Optimize the data layer** — for relational systems like Oracle, partitioning and indexing for query performance. For OpenSearch, time-based indices and proper shard count for parallel execution.
>
> **For heavy workloads, asynchronous processing** via Kafka or SQS so the system absorbs spikes without impacting user-facing latency.
>
> **Trade-offs**
>
> Stateless services scale easily at the application layer, but the data layer still has consistency-vs-availability trade-offs. Async improves scalability but introduces eventual consistency. More partitions improve performance but add operational complexity.

---

## Q9: Multi-layer scaling approach

**Memory Hook:** Edge → API Gateway → Compute → Data → Async

> **Core Answer**
>
> ```
> Edge          → CDN for static, caching at edge
> API Gateway   → Routing, throttling, security
> Compute       → Kubernetes HPA (Horizontal Pod Autoscaler), cloud auto-scaling
> Data          → Partitioning, indexing, sharding for horizontal scale
> Async         → Kafka / SQS for decoupling and spike absorption
> ```
>
> Each layer is independently scalable. The discipline is **scaling the constraint, not everything**. If the bottleneck is the data layer, throwing more pods at the API layer wastes money without helping latency.

---

## Q10: Scale from 120 to 1200 customers

**Memory Hook:** Diagnose → Quick Wins → Horizontal Scaling → Data Layer Scaling → Operational Readiness → Team Scaling → Incremental Rollout

> **Discipline Rule**
>
> **Critical lesson from Ananth at Availity:** he redirected this question 3 times when the answer kept going back to infrastructure. **Lead with team and operational readiness. Infrastructure last.**
>
> **Scaling 10x is primarily organizational and operational, not just technological.** Cloud handles compute elasticity — that's the easy part.

> **Core Answer**
>
> **Step 1 — Diagnose.** Profile the actual bottleneck. DB? A specific service? Network? **Scale the constraint, not everything.**
>
> **Step 2 — Quick wins before re-architecture.** Redis caching for read-heavy data. Connection pool tuning (HikariCP). Query optimization with execution plans and composite indexes. CDN for static assets.
>
> **Step 3 — Horizontal scaling.** Stateless services + Kubernetes HPA. Load balancer tuning. AKS/EKS node pool scaling.
>
> **Step 4 — Data layer scaling.** Read replicas. Tenant-based partitioning in Oracle. Index-per-tenant or time-based in OpenSearch. S3 lifecycle policies for cold data.
>
> **Step 5 — Operational readiness.** SLOs defined per service **before next customer cohort**. Alerting on p99 latency, error rate, Kafka consumer lag. Runbooks for every tier-1 failure mode.
>
> **Step 6 — Team scaling.** Domain-based ownership. Platform team for shared infrastructure. **30–40% headcount growth for 10x customer growth** (not 1:1 — established teams are more efficient).
>
> **Step 7 — Incremental rollout.** Scale one layer at a time. Load test before each major customer onboarding. Canary deployments for risky changes.
>
> **Example (Cerner)**
>
> When Care Management grew customer count, **OpenSearch latency climbed from 80ms to 700ms** on readmission risk queries. Root cause: **hot shards** because all tenants shared one index.
>
> We introduced **tenant-based index routing and time-based rollover — latency back to 95ms**.
>
> For the DB layer, connection pool exhaustion was next — we tuned **HikariCP** and added **Redis cache for reference data, cutting Oracle load by 40%**.
>
> **AKS HPA on API pods** scaled them automatically during peak alert windows. Each step was load-tested before the next customer cohort onboarded.

---

## Q11: Design a high-throughput system

**Memory Hook:** Edge → Application → Data → Resilience

> **Core Answer**
>
> Remove bottlenecks across all layers.
>
> **Edge.** CDN and API gateway for traffic distribution, caching, rate limiting.
>
> **Application.** Stateless services scale horizontally. Async processing for heavy workloads — user requests not blocked.
>
> **Data.** Critical for throughput. Oracle: partitioning, indexing, query optimization. OpenSearch: time-based partitioning, appropriate shard counts for parallel execution.
>
> **Resilience.** Circuit breakers, retries with backoff, observability to detect and recover from failures.
>
> **Trade-offs**
>
> Aggressive caching improves throughput but introduces stale data. Heavy sharding improves performance but increases operational overhead.
>
> **Example**
>
> Improved throughput at Cerner by introducing **Redis caching, moving heavy workflows to Kafka async, and optimizing OpenSearch shard configuration**. Significantly improved capacity and reduced response times under load.

---

# SECTION C — DISTRIBUTED SYSTEM PATTERNS

---

## Q12: Backpressure handling

**Memory Hook:** Rate Limiting → Circuit Breaker → Queue Buffering → Reactive Streams → Bulkhead

> **Simple analogy:** Water tank fills at 100L/min, pipe to bucket empties at 20L/min. Without flow control, bucket overflows. Backpressure tells the tank to slow down.

> **Core Answer**
>
> Backpressure is primarily about **controlling overload before the system collapses**. I start with rate limiting and bounded buffering, followed by reactive backpressure or flow-control mechanisms. Then isolation patterns like bulkheads, and resiliency patterns like timeouts, circuit breakers, retries, and fallbacks — applied carefully to avoid retry storms worsening the overload.
>
> **Five strategies**
>
> **1. Rate Limiting — reject early at the gate**
> ```java
> RateLimiter.of("api", RateLimiterConfig.custom()
>     .limitForPeriod(1000)
>     .limitRefreshPeriod(Duration.ofSeconds(1))
>     .build());
> // Exceeds limit → 429 Too Many Requests — clean, fast failure
> ```
>
> **2. Circuit Breaker — stop calling a failing service**
> ```
> CLOSED (normal) → OPEN (too many failures, fast fail) → HALF_OPEN (test recovery)
> ```
> ```java
> CircuitBreaker.of("inventory", CircuitBreakerConfig.custom()
>     .failureRateThreshold(50)          // open if 50% fail
>     .waitDurationInOpenState(Duration.ofSeconds(30))
>     .build());
> ```
>
> **3. Queue Buffering — absorb the spike**
> Kafka between producer and consumer. Consumer pulls at its own pace. Monitor consumer lag in New Relic — alert when lag exceeds threshold.
>
> **4. Reactive Streams — demand-based flow control**
> ```java
> Flux.fromIterable(events)
>     .onBackpressureBuffer(1000)
>     .flatMap(event -> process(event), 16)  // max 16 concurrent
>     .subscribe();
> ```
>
> **5. Bulkhead — isolate failures to a pool**
> ```java
> Bulkhead.of("opensearch", BulkheadConfig.custom()
>     .maxConcurrentCalls(20).build());
> // OpenSearch slow → 20 threads affected → other services unaffected
> ```
>
> **Production layering**
> ```
> Request → Bulkhead → TimeLimiter → CircuitBreaker → Retry → call → Fallback
> ```

---

## Q13: Data consistency in distributed systems

**Memory Hook:** Strong Consistency for Correctness | Eventual Consistency for Scale | Saga + Outbox for Workflow

> **Core Answer**
>
> Apply consistency **based on operation criticality, not uniformly.**
>
> **Financial transactions or critical clinical decisions** — strong consistency. Correctness cannot be compromised.
>
> **Analytics, notifications, reporting** — eventual consistency. Slightly stale data is acceptable if throughput and availability are higher.
>
> **Cross-service workflows** — Saga pattern. Reliable event publishing — Outbox pattern. Idempotency across all async operations.
>
> **Trade-off**
>
> CAP theorem is real: in distributed systems during network partition you choose between consistency and availability. **Make this choice explicitly per use case, not by accident.**

---

## Q14: SAGA Pattern — Choreography vs Orchestration

**Memory Hook:** Saga for Distributed Transactions | Choreography for Simple Flows | Orchestration for Critical Workflows

> **Core Answer**
>
> I use Saga when managing transactions across multiple services in a distributed system, where traditional ACID transactions are not feasible.
>
> The workflow is broken into a series of local transactions. **If any step fails, compensating actions trigger to maintain consistency.**
>
> **Choreography** — services communicate through events and react independently. Loose coupling. Works well for simple flows. Hard to track and debug as the system grows.
>
> **Orchestration** — central orchestrator controls the sequence and handles failures explicitly. Better visibility and control. Tighter coupling.
>
> **My preference for critical workflows: orchestration.** Better debugging. Explicit error handling. Worth the central dependency.
>
> **Example (Cerner)**
>
> Simple workflows like notifications used **choreography**. Critical workflows like **patient processing → risk evaluation → notification** used an **orchestrator** for explicit control.
>
> **Compensating actions example**
> ```java
> // Step 1: Reserve inventory  →  SUCCESS: proceed
> // Step 2: Create pick task   →  FAIL: COMPENSATE Step 1 (release reservation)
> // Step 3: Generate shipment  →  FAIL: COMPENSATE Steps 1 & 2
>
> void releaseInventoryReservation(String reservationId) {
>     inventory.updateStatus(reservationId, RELEASED);
>     outboxRepo.save(new OutboxEvent("INVENTORY_RELEASED", reservationId));
> }
> ```

---

## Q15: Outbox Pattern — Why SAGA Alone Is Not Enough

**Memory Hook:** DB Commit ✅ Event Publish ❌ → Outbox

> **Core Answer**
>
> Common consistency problem: **DB transaction succeeds but event publication to Kafka fails.** Mismatch between system state and downstream consumers — a "ghost state."
>
> **Without Outbox:**
> ```java
> @Transactional
> void reserveInventory() {
>     repo.save(reservation);                     // committed
>     kafkaTemplate.send("events", payload);      // CRASH HERE
>     // DB committed, event never sent. Ghost state.
> }
> ```
>
> **With Outbox:** write the event to the DB in the same transaction as the state change.
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
> **Trade-offs**
>
> Improves reliability and consistency but adds complexity managing the outbox table and background processing. Slight latency in event propagation since events publish asynchronously.
>
> **To strengthen,** combine with: idempotent consumers, retry with exponential backoff, DLQ for persistent failures, and CDC tools like Debezium for streaming outbox events.

---

## Q16: Payment service failure — design response

**Memory Hook:** Fail Fast → Retry With Backoff → Circuit Breaker → Saga Compensation

> **Core Answer**
>
> When payment fails, the priority is **system stability while maintaining good user experience.**
>
> **Fail fast** — do not block the system. Prevents cascading failures.
>
> **Retry with backoff** — exponential backoff for transient failures.
>
> **Circuit breaker** — stop repeated calls to a failing service to avoid overwhelming it.
>
> **User feedback** — clear feedback. Allow user to retry or show meaningful failure state. **Never leave system in uncertain state.**
>
> **Saga compensation** — for distributed workflows. If payment fails after order creation, trigger compensating actions (cancel order, release inventory).
>
> **Trade-offs**
>
> Retries improve success but increase latency. Circuit breakers protect but may temporarily reject valid requests. Compensation ensures consistency but adds workflow complexity.
>
> **To strengthen:** idempotency keys to prevent duplicate payments, DLQ for failed events, monitoring for failure rates, graceful degradation when providers are unavailable.

---

## Q17: How do you design for failure?

**Memory Hook:** Assume Failures → Isolation → Recovery → Graceful Degradation

> **Core Answer**
>
> I design assuming failures will happen, especially in distributed environments.
>
> **Isolation** — failures in one component don't cascade. Service boundaries, bulkheads, circuit breakers.
>
> **Recovery** — for transient failures, retries with exponential backoff. For persistent failures, fallback so the system continues operating in limited capacity.
>
> **Graceful degradation** — instead of failing completely, provide partial functionality. Serve cached data. Disable non-critical features.
>
> **Observability** — strong monitoring with metrics, logs, tracing. Alerts so failures are detected early.
>
> **Trade-offs**
>
> Resilience adds complexity. Overusing retries can amplify load during failures.
>
> **Example**
>
> Downstream service failures were impacting user-facing APIs at Cerner. We introduced **circuit breakers and fallback responses, plus retries for transient issues**. System remained stable even when dependencies were partially unavailable.

---

## Q18: How do you design for high availability?

**Memory Hook:** No Single Point of Failure → Replication → Failover → Multi-Region

> **Core Answer**
>
> Ensure **no single point of failure** across any layer.
>
> **Infrastructure** — deploy across multiple availability zones, load balance across them. If one AZ fails, traffic routes to healthy zones automatically.
>
> **Application** — stateless services + Kubernetes health checks + auto-healing. Unhealthy pods replaced without manual intervention.
>
> **Data** — replication. Read replicas in Oracle, replica shards in OpenSearch. Data available even if primary fails.
>
> **Failover** — automatic within region. For critical systems, active-active multi-region with global load balancing.
>
> **Trade-offs**
>
> Multi-region adds cost, cross-region consistency complexity, and latency. Justify based on RTO/RPO requirements — **not every system needs multi-region.**
>
> For critical systems, also consider: active-active vs active-passive multi-region, RTO/RPO planning, DNS-based or global load balancing for failover, regular failover testing.

---

## Q19: Active-Active design — what happens when the local DB goes down mid-transaction?

**Memory Hook:** Local Atomicity → User Sees Failed Request → Retry Routes to Healthy Zone → Eventual Catch-up Via Kafka

> **Discipline Rule**
>
> This came up at TJX. You took 3 turns to land it. **Memorize this clean answer.**

> **Core Answer**
>
> If the local DB write and the outbox table write don't both succeed, the `@Transactional` rolls back — **atomicity is preserved at the local zone level**. The user sees a failed request.
>
> On retry, traffic may route to the healthy zone where the transaction completes. **Worst case is brief unavailability for the affected users — not data inconsistency.**
>
> Eventual consistency between zones is handled by **Kafka-based replication of outbox events**. Once the failed zone recovers, it catches up via the same Kafka topic.
>
> **Two key design points:**
> - **Atomicity is local** — guaranteed by the transactional boundary in the failing zone
> - **Cross-zone consistency is eventual** — guaranteed by the outbox + Kafka pattern
>
> **What I would NEVER do:** try to write across zones synchronously to mask the failure. That breaks the active-active model and creates worse failure modes than the one we're trying to handle.
>
> **Variant — TJX-style stateful mediation layer**
>
> If the application is a **stateful mediation layer** (like the TJX Manhattan WMS context) where in-flight transactions must finish in the same zone they started:
>
> - **Sticky session routing** at the application layer (not just geolocation)
> - **Transaction affinity** — once a transaction starts in zone A, all subsequent calls go to zone A until commit
> - **DR fallback only for new transactions**, not for in-flight ones
>
> *"For a mediation layer with stateful in-flight transactions, geolocation routing is the wrong abstraction. You want session/transaction affinity at the application layer. The active-active design protects against zone failure for new traffic — not for transactions already in progress."*

---

# SECTION D — SPECIFIC SYSTEM DESIGNS

---

## Q20: Design a Notification System

**Memory Hook:** Business Event → Kafka Topic → Notification Service → Channel → Retry → Status Callback

> **Core Answer — Architecture**
>
> ```
> Business Event (order placed, patient alert)
>     │
>     ▼
> Kafka Topic (notification-events)
>     Producer includes: correlation ID, event type, recipient, channel preference
>     │
>     ▼
> Notification Service (consumer)
>     Determines channel: email / SMS / push / in-app
>     Sends via channel provider
>     Updates status: PENDING → SENT / FAILED
>     │
>     ├── Success: publish to status topic with correlation ID
>     └── Failure: retry 3x with exponential backoff → DLQ
>
> Producer tracking:
>     → Subscribe to status topic with correlation ID
>     → OR poll GET /notifications/{correlationId}/status
> ```
>
> **How do you respond back with success/failure status?**
>
> **Pattern 1 — Async callback (preferred):** When a producer submits a notification request, they include a correlation ID and a reply-to Kafka topic. When our notification service finishes — success or failure — we publish the result to that reply-to topic with the same correlation ID. The producer subscribes and matches.
>
> **Pattern 2 — Polling endpoint (for sync consumers):** `GET /notifications/{correlationId}/status` returns `PENDING`, `DELIVERED`, `FAILED`, or `DEAD_LETTERED`. State stored in a status table that the notification service updates on every transition.
>
> **Pattern 3 — Permanent failures:** After retries exhaust, the message lands in DLQ with error code and reason. Operations alerted. We expose a replay endpoint for manual intervention after fixing root cause.
>
> **Key design decisions**
>
> - **At-least-once delivery with idempotent processing** — not exactly-once (too complex, unnecessary)
> - **Rate limiting per channel provider** — email vendors have rate caps
> - **User preferences** — opt-in/opt-out stored in preferences service
> - **DLQ for permanent failures** — ops reviews daily, replay after fix

---

## Q21: Scale notification service from 1M to 5M orders/day

**Memory Hook:** Pod Scaling → Partition Scaling → Provider Rate Limits

> **Core Answer**
>
> Two layers — processing and messaging.
>
> **Processing.** Kubernetes HPA on CPU at **70% (not 80%** — scaling takes time, you want headroom). Min and max pod counts based on observed traffic. For seasonal spikes, **pre-scale before the event** rather than reacting.
>
> **Messaging.** Scale Kafka consumer group partitions. Each consumer reads from one partition. 1M to 5M means **5x consumer throughput** — increase partition count accordingly. Kafka handles buffering automatically.
>
> **Key parameters to watch**
>
> Consumer lag in Kafka, pod CPU saturation, **email provider API rate limits — the external vendor is often the actual bottleneck at 5M/day**, not our own service.
>
> **Cost optimization**
>
> Spot instances or preemptible VMs for burst capacity — stateless consumer, failures restart from last committed Kafka offset.

---

# SECTION E — DATA STORE DESIGN (PARTITIONING & SHARDING)

---

## Q22: Partitioning vs Sharding — clean definitions

> **Discipline Rule**
>
> **This is the question Cubic asked. The wrong answer cost grade. Memorize the correct definitions.**

**Memory Hook:** Partitioning = ONE machine | Sharding = MANY machines

> **Core Answer**
>
> **Partitioning** — splitting data **within ONE database instance** by a logical key. For example, partitioning a patient table by `tenant_id` so each tenant's data is in a separate partition. Reads for one tenant do not touch another tenant's partition.
>
> **PostgreSQL example:**
> ```sql
> CREATE TABLE orders (
>     order_id   SERIAL,
>     order_date DATE NOT NULL,
>     amount     NUMERIC
> ) PARTITION BY RANGE (order_date);
>
> CREATE TABLE orders_2024 PARTITION OF orders
>     FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
> ```
>
> Three strategies: **RANGE** (dates, IDs), **LIST** (country, status), **HASH** (even distribution).
>
> **Sharding** — distributing partitions **ACROSS MULTIPLE physical database instances or nodes**. Each shard is a separate DB server holding a subset of total data. Horizontal scaling at the data layer — no single instance is the bottleneck. **Cross-shard queries become complex; you need a routing layer.**
>
> **Clean rule of thumb (memorize)**
>
> - **Partitioning** = splitting data on **one machine**
> - **Sharding** = splitting data across **many machines**
> - **Sharding is partitioning at distributed scale** — a superset
>
> **OpenSearch — sharded by default**
> ```json
> PUT /patient_records
> {
>   "settings": {
>     "number_of_shards": 3,
>     "number_of_replicas": 1
>   }
> }
> ```
> Routing formula: `shard = hash(document_id) % number_of_primary_shards`
>
> Shard count is **fixed at index creation** — hard to change without reindexing. Replicas add fault tolerance, not additional primary storage.
>
> **RDBMS sharding options**
> - **Citus** extension for PostgreSQL — distributes tables across nodes
> - **Vitess** — sharding middleware for MySQL (used by YouTube, GitHub)
> - **Application-level sharding** — app decides which DB server based on key
>
> **OpenSearch partitioning options**
> - **Index-per-tenant** — one index per customer
> - **Time-based indices** — `orders-2024`, `orders-2025` (prune by index, not by scan)
> - **Custom routing keys** — force related documents to same shard

---

## Q23: Database access patterns

**Memory Hook:** Datastore Dictates Pattern

> **Core Answer**
>
> Depends on the datastore.
>
> **OpenSearch** — index design, correct field mapping, query optimization (filters, avoiding deep pagination), sharding strategy.
>
> **Oracle** — indexing strategy, partitioning, query optimization via execution plans, reducing DB calls through caching.
>
> **Example (Cerner)**
>
> Queries slowed due to **large table scans on a patient processing system**. Fix: added **composite indexes, introduced date-based partitioning**. Query time reduced from seconds to milliseconds.

---

# SECTION F — JAVA / SPRING BOOT MICROSERVICES (JPMC STACK)

> *The JD calls out Java/Spring Boot, REST APIs, and microservices specifically. Be ready to discuss them at design and operational level — not just syntax.*

---

## Q24: How do you design REST APIs and Spring Boot microservices for production?

**Memory Hook:** Contract First → Service Boundaries → Resilience → Observability → Secure by Default

> **Core Answer**
>
> Five disciplines I enforce on every Spring Boot service.
>
> **1. API Governance — Contract First.** Every API is defined using **OpenAPI** and goes through design review before any code is written. We treat APIs as products, not implementation details. Versioning is explicit — `/v1`, `/v2` — with a strict no-breaking-changes policy. If a change is unavoidable, formal deprecation cycle and support N-1 versions. This prevents downstream failures and gives consumer teams confidence to integrate.
>
> **2. Domain Integrity — Clear Service Boundaries.** I align services with **bounded contexts** to ensure high cohesion (one component does one focused job well) and low coupling. For example, in an account origination domain, I would separate: application-intake, KYC, risk scoring, decisioning. Each service owns its own database, and I enforce a hard rule: **no cross-service database joins.** If shared data is needed, use APIs or async events.
>
> **3. Engineering for Failure — Resilience.** Infrastructure will fail; the application must be defensive. I use **Resilience4j** for circuit breakers, bulkheads, and retries with exponential backoff. The goal is to prevent a single downstream delay from cascading into a total system blackout — an anti-fragile approach. Systems should degrade gracefully, not fail catastrophically.
>
> **4. Observability — No Dark Corners.** A service is invisible without telemetry. I bake in **Spring Boot Actuator and Micrometer from day one**, with structured JSON logging and **correlation IDs propagated through every hop**. My policy: **no dashboard, no deploy.**
>
> **5. Security — Secure by Default.** Security isn't a perimeter; it's a layer. **OAuth2/JWT validated at the API Gateway and re-verified at the service level (Zero Trust).** Beyond that: strict **Bean Validation** on all inputs, secrets in a vault (never in properties files), and audit logging for sensitive operations.

---

## Q25: How do you decide REST vs Kafka for inter-service communication?

**Memory Hook:** Synchronous for Queries → Asynchronous for State Changes → Saga for Distributed Transactions

> **Core Answer**
>
> Three rules.
>
> **Synchronous for Queries (REST/gRPC).** If a consumer cannot proceed without an immediate response, use REST. This is typical for 'Read' operations — for instance, an Origination Service fetching a customer's credit score in real-time to render a UI. **The latency is the price we pay for immediate consistency.** Enforce strict timeouts and circuit breakers so a slow downstream query doesn't hang the upstream service.
>
> **Asynchronous for "Fan-Out" State Changes (Kafka).** For 'Write' operations or business events, default to Kafka. When an application is submitted, multiple downstream systems (KYC, Risk, Email) need to react. **Using REST here creates a "distributed monolith"** where a failure in the Email service could crash the entire submission flow. Kafka decouples these; the producer fires the event and moves on, consumers process at their own pace.
>
> **Distributed Transactions (Saga + Outbox).** When a business process spans multiple services — like an Account Origination workflow — move to a **Saga Pattern**.
> - **Choreography** for simpler flows: services listen and react to each other's events.
> - **Orchestration** for complex logic: an orchestrator manages the sequence and compensating transactions if a step fails.
>
> To bridge the gap between DB and Kafka, **mandate the Transactional Outbox Pattern** (Q15). This ensures we never commit a DB change without successfully emitting the corresponding Kafka event, preserving data integrity even during a crash.

---

## Q26: How do you ensure Spring Boot service performance and tune it?

**Memory Hook:** Measure First → Persistence Hot Path → Intelligent Caching → Tune JVM → Async Where It Fits

> **Core Answer**
>
> Five-step approach.
>
> **1. Baselines & APM (Measure First).** Never tune without a baseline. Use APM (New Relic, Dynatrace) to identify the 'Top 5' slowest transactions. **Focus on p95 and p99, not averages** — those represent actual user friction. Also watch error rates; often poor performance is a symptom of retries from hidden errors. **No optimization without data.**
>
> **2. Persistence Layer (Hot Path Optimization).** **90% of Spring Boot performance issues are in the data layer.** Look for N+1 problems in Hibernate, missing indexes, expensive full-table scans. Use `EXPLAIN ANALYZE` to ensure queries are efficient. Enforce Lazy Loading by default, Eager Fetching specifically for hot paths to reduce DB round-trips.
>
> **3. Intelligent Caching (Cache Deliberately).** Redis for shared cache, Caffeine for in-process hot data. Always with TTL and clear invalidation. **Cache without eviction is a memory leak waiting to happen** — I caught one at Cerner that **improved memory by 40%** after fixing.
>
> **4. Tune JVM.** Right heap size based on observed allocation. **G1GC** default for most workloads. Tomcat thread pools sized to actual concurrency. **Don't tune blind** — change one variable, measure, repeat.
>
> **5. Async where it fits.** For I/O-bound services, use `@Async` or `CompletableFuture` to free up main request threads. For massive scale (notification engine), consider Spring WebFlux (Project Reactor). **But cautious here** — reactive adds significant cognitive complexity. Only approve it when high I/O concurrency is the proven bottleneck.

---

## Q27: How do you secure REST APIs in a banking context?

**Memory Hook:** Authentication → Authorization → Input Validation → Encryption → Audit Logging

> **Core Answer**
>
> Five layers.
>
> **Authentication** — OAuth2/OIDC with JWT, validated at the API gateway and **re-validated at each service** for defense-in-depth. No anonymous access on internal APIs.
>
> **Authorization** — RBAC and attribute-based controls via Spring Security. Method-level annotations for fine-grained checks. **Principle of least privilege.**
>
> **Input validation** — Bean Validation at the controller. **Reject bad input at the edge, never trust downstream.** SQL injection, XSS, schema violations — all caught here.
>
> **Encryption** — TLS in transit, AES-256 at rest, secrets in **HashiCorp Vault** or equivalent. **Never in config files**, never in environment variables for sensitive secrets.
>
> **Audit logging** — every privileged action (view, modify, approve) logged with **who, what, when, correlation ID**. Required for SOX, PCI, and regulator inquiries.

---

## Q28: How do you handle errors and exceptions consistently across services?

**Memory Hook:** Centralize With Global Interceptors → Standardize Via RFC 7807 → Zero-Leakage Policy

> **Core Answer**
>
> To build a mature microservices ecosystem, you cannot have engineers "improvising" error responses. I enforce a strict, three-tier strategy for consistency, security, and traceability.
>
> **1. Centralize with Global Interceptors.** Move error handling out of business logic. Mandate **`@ControllerAdvice` and `@ExceptionHandler`** in Spring Boot. Keeps controllers clean and noise-free. The service layer throws custom functional exceptions (e.g. `OrderNotFoundException`); global advice handles translation to HTTP status codes.
>
> **2. Standardize via RFC 7807 (Problem Details).** Consistency is key for consumer contracts. Every service returns a structured JSON body containing:
> - **Type & Title** — categorizes the error
> - **Status** — the HTTP code
> - **Detail** — human-readable explanation (safe for clients)
> - **Correlation ID** — echo the unique request ID back to the client so support can immediately find the exact trace
>
> **3. Zero-Leakage Security Policy.** Hard ban on leaking internal system details. Stack traces, DB schema names, internal paths **never reach the client** — significant security vulnerability.
> - **Internal:** log full technical detail in the aggregator
> - **External:** client receives a sanitized, generic message
>
> Precise HTTP mapping — **don't lazy-load every error into 500**. Map validation failures to 400, permission issues to 403, missing resources to 404. Allows API Gateway and monitoring tools to accurately distinguish user errors from system failures.
>
> **My mantra:** *"Logs are for engineers; responses are for consumers."*

---

# SECTION G — KAFKA & EVENT-DRIVEN ARCHITECTURE (JPMC STACK)

> *The JD calls out Kafka producers, consumers, and streaming pipelines for real-time data processing. Be ready for both design and operational depth.*

---

## Q29: How do you design Kafka topics, partitions, and retention?

**Memory Hook:** Partitions → Partition Key → Replication Factor → Retention

> **Core Answer**
>
> Four decisions for every topic.
>
> **Partitions** — set based on expected peak throughput and consumer parallelism. **More partitions = more parallel consumers**, but rebalancing overhead grows too. Start with what you need; **over-partitioning is hard to undo.**
>
> **Partition key** — chosen carefully. For account events, `account-id` gives ordering per account but can hot-spot if traffic is skewed. For events where global ordering doesn't matter, hash distribution is fine.
>
> **Replication factor** — **3 in production with `min.insync.replicas=2`**. Tolerates one broker failure without data loss. Non-negotiable for tier-1 topics.
>
> `min.insync.replicas=2` means Kafka requires at least two in-sync replicas to acknowledge a write before considering it successful when the producer uses `acks=all`. This improves durability by preventing writes from succeeding with only a single replica available.
>
> **Retention** — operational topics 3–7 days. Audit and replay topics 30+ days, or **compacted indefinitely**. For latest-state topics (e.g. customer profile snapshot), **log compaction** keeps the latest value per key forever.

---

## Q30: How do you ensure exactly-once or at-least-once delivery?

**Memory Hook:** Idempotent Producer → Transactions for Atomic Writes → Idempotent Consumer

> **Core Answer**
>
> Three layers.
>
> **Idempotent producer.** `enable.idempotence=true` prevents duplicate writes from retries within Kafka itself.
>
> **Transactions for atomic multi-topic writes.** When a single business event needs to land in two topics atomically — say, `account.created` and `audit.event` — wrap in a Kafka transaction. Same for read-process-write patterns.
>
> **Idempotent consumer is the realistic guarantee.** **True end-to-end exactly-once across producer, broker, consumer, and downstream sink is rare in practice.** Instead, design consumers to be idempotent — use a **deduplication key (event ID)** and check before processing. **UPSERT, not blind INSERT.** Operations safe to repeat.
>
> The **Outbox pattern (Q15)** closes the gap when a producer crashes between DB commit and Kafka send.

---

## Q31: How do you handle errors and dead letter queues in Kafka consumers?

**Memory Hook:** Categorize Errors → Retry Transient → DLQ Permanent → Replay → Alert on Growth

> **Core Answer**
>
> Five-step pipeline.
>
> **Categorize errors first.** Transient (network, downstream timeout) vs permanent (bad payload, schema violation). **Different errors need different handling.**
>
> **Retry transient errors with backoff.** Local retry first (one or two attempts), then a **dedicated retry topic with delay** if it persists.
>
> **DLQ for permanent failures.** Send the original payload, error, timestamp, and headers to a dead-letter topic. **Don't lose data; quarantine it.**
>
> **Replay capability.** Tooling to push DLQ messages back into the main topic after the fix is deployed. **DLQ is a triage queue, not a graveyard.**
>
> **Alert on DLQ growth.** Threshold-based alert. **If DLQ is filling up, something is broken upstream.**

---

## Q32: How do you monitor Kafka pipelines in production?

**Memory Hook:** Consumer Lag → Throughput → Error Rate → Cluster Health → Alert on SLO Breach

> **Core Answer**
>
> Five signals.
>
> **Consumer lag.** **The single most important metric.** Lag growth means consumers can't keep up. Alert when lag exceeds your data-freshness SLO.
>
> **Throughput.** Messages per second per topic, per consumer group. **Sudden drops are leading indicators of problems.**
>
> **Error rate.** Failed messages, deserialization errors, retries, DLQ writes. **Trend matters more than absolute number.**
>
> **Cluster health.** Under-replicated partitions, ISR shrinks, broker disk usage. Platform-team responsibility, but I monitor as a consumer of the platform.
>
> **Alert on SLO breach, not infra noise.** Lag > X minutes, error rate > Y%, throughput drop > Z%. **Pages should be actionable.**

---

## Q33: How do you handle Kafka schema evolution?

**Memory Hook:** Schema Registry → Compatibility Rules → Additive Changes Only → Version Explicitly When Breaking

> **Core Answer**
>
> Four practices.
>
> **Schema Registry.** Confluent or equivalent. All schemas versioned, validated, and central. Producers and consumers register what they expect.
>
> **Compatibility rules.** **BACKWARD by default** — new consumers can read old data. **FULL for shared, critical topics** — old and new consumers coexist.
>
> **Additive changes only.** Add optional fields with defaults. **Never remove or rename existing ones.** Most "breaking changes" can be done as additive.
>
> **Version explicitly when truly breaking.** Create `account.created.v2`. Run both topics in parallel during deprecation. **Monitor v1 consumer count. Retire only when truly unused — not when you wish it was unused.**

---

## Q34: Design a Kafka-driven event flow for an account origination journey

**Memory Hook:** Capture → Orchestrate → Aggregate → Notify → Audit

> **Core Answer**
>
> Five stages, all event-driven.
>
> **Capture.** `application.submitted` is published when the customer completes the form. **Single source of truth for downstream.**
>
> **Orchestrate downstream work.** KYC service consumes and runs identity check. Document service consumes and processes uploads. Risk service consumes and runs scoring. **Each emits its own completion event** — `kyc.completed`, `documents.verified`, `risk.cleared`.
>
> **Aggregate state.** An orchestration service consumes the per-step events and emits `application.approved` or `application.rejected` based on rules. **This is where Saga choreography lives.**
>
> **Notify and update UI.** Notification service consumes the final state event and sends email/SMS. UI subscribes via WebSocket or polls a status endpoint backed by the consolidated event.
>
> **Audit trail.** All events retained in a **compacted audit topic** for compliance, replay, and analytics. Required for regulator inquiries — bank's reality.
>
> **Three reliability anchors**
>
> Outbox pattern for guaranteed event publication, idempotent consumers for safe retries, DLQ + replay for poison messages.

---

## Q35: OAuth 1.0 vs OAuth 2.0

**Memory Hook:** OAuth 1.0 = Signed Courier Package | OAuth 2.0 = Hotel Access Card

> **Core Answer**
>
> Two different security models, with OAuth 2.0 being the dominant standard today.
>
> **OAuth 1.0 — "Signed Courier Package"**
>
> Every request must include a signature, tamper-proof seal, and verification stamp. Each call cryptographically signed with a shared secret. Even if intercepted, the request cannot be modified without invalidating the signature. **Strong tamper protection, but heavy on every call.**
>
> **OAuth 2.0 — "Hotel Temporary Access Card"**
>
> Client receives a temporary access token (the "card"). The token grants specific scopes (room access, gym access) and expires after a defined time. Client doesn't carry the master credential. **Lighter weight, relies on HTTPS for transport security rather than per-message signing.**
>
> **Trade-offs**
>
> OAuth 2.0 is simpler to implement and dominant in the industry, but transport security depends on TLS. OAuth 1.0 is more complex to implement but provides per-message integrity. In modern banking and enterprise systems, **OAuth 2.0 with OIDC and JWTs** — validated at gateway and re-validated at service — is the standard pattern, combined with strong TLS, mTLS where needed, and short token lifetimes.

---

# QUICK REFERENCE — MEMORY HOOKS

| # | Topic | Memory Hook |
|---|---|---|
| Q1 | Approach to system design | Clarify Problem → Break Into Layers → Evaluate Options → Evaluate Trade-offs → Align With Enterprise Standards |
| Q2 | Monolith vs microservices | Team Structure + Domain Complexity + Scaling Granularity → Architecture Choice |
| Q3 | Strangler Pattern | Gradual Replacement → Reduce Operational Risk → Continuous Business Delivery |
| Q4 | 12-Factor App | Stateless Processes → Config Via Environment → Logs As Streams → Backing Services Attached → Dev/Prod Parity |
| Q5 | When systems scale | Technology → Operational Maturity → Governance → Process and Team Structure |
| Q6 | Governance vs Review | Review = Point-in-Time / Governance = Continuous |
| Q7 | Modernize legacy | Assess → Decompose Strategically → Incremental Cutover → Govern the Transition |
| Q8 | Design for scalability | Stateless Services → Distribute Traffic → Optimize Data Layer → Asynchronous Processing |
| Q9 | Multi-layer scaling | Edge → API Gateway → Compute → Data → Async |
| Q10 | Scale 120 → 1200 customers | Diagnose → Quick Wins → Horizontal Scaling → Data Layer Scaling → Operational Readiness → Team Scaling → Incremental Rollout |
| Q11 | High-throughput design | Edge → Application → Data → Resilience |
| Q12 | Backpressure | Rate Limiting → Circuit Breaker → Queue Buffering → Reactive Streams → Bulkhead |
| Q13 | Distributed consistency | Strong Consistency for Correctness / Eventual Consistency for Scale / Saga + Outbox for Workflow |
| Q14 | SAGA choreo vs orchestration | Saga for Distributed Transactions / Choreography for Simple Flows / Orchestration for Critical Workflows |
| Q15 | Outbox Pattern | DB Commit ✅ Event Publish ❌ → Outbox |
| Q16 | Payment failure | Fail Fast → Retry With Backoff → Circuit Breaker → Saga Compensation |
| Q17 | Design for failure | Assume Failures → Isolation → Recovery → Graceful Degradation |
| Q18 | High availability | No Single Point of Failure → Replication → Failover → Multi-Region |
| Q19 | Active-Active DB failure | Local Atomicity → User Sees Failed Request → Retry Routes to Healthy Zone → Eventual Catch-up Via Kafka |
| Q20 | Notification system | Business Event → Kafka Topic → Notification Service → Channel → Retry → Status Callback |
| Q21 | Scale notifications 1M→5M | Pod Scaling → Partition Scaling → Provider Rate Limits |
| Q22 | Partitioning vs Sharding | Partitioning = ONE machine / Sharding = MANY machines |
| Q23 | DB access patterns | Datastore Dictates Pattern |
| Q24 | Spring Boot prod design | Contract First → Service Boundaries → Resilience → Observability → Secure by Default |
| Q25 | REST vs Kafka | Synchronous for Queries / Asynchronous for State Changes / Saga for Distributed Transactions |
| Q26 | Spring Boot tuning | Measure First → Persistence Hot Path → Intelligent Caching → Tune JVM → Async Where It Fits |
| Q27 | Secure REST (banking) | Authentication → Authorization → Input Validation → Encryption → Audit Logging |
| Q28 | Error handling | Centralize With Global Interceptors → Standardize Via RFC 7807 → Zero-Leakage Policy |
| Q29 | Kafka topics/partitions | Partitions → Partition Key → Replication Factor → Retention |
| Q30 | Exactly-once vs at-least-once | Idempotent Producer → Transactions for Atomic Writes → Idempotent Consumer |
| Q31 | Kafka DLQ | Categorize Errors → Retry Transient → DLQ Permanent → Replay → Alert on Growth |
| Q32 | Monitor Kafka | Consumer Lag → Throughput → Error Rate → Cluster Health → Alert on SLO Breach |
| Q33 | Kafka schema evolution | Schema Registry → Compatibility Rules → Additive Changes Only → Version Explicitly When Breaking |
| Q34 | Account origination flow | Capture → Orchestrate → Aggregate → Notify → Audit |
| Q35 | OAuth 1.0 vs 2.0 | OAuth 1.0 = Signed Courier Package / OAuth 2.0 = Hotel Access Card |

---

# APPENDIX A — DISTRIBUTED SYSTEM PATTERNS QUICK REFERENCE

| Pattern | One-Line | Use When |
|---------|----------|----------|
| **SAGA** | Distributed transaction with compensating ops | Multi-service workflow without single DB transaction |
| **Outbox** | Event written in same DB tx as state change | Guarantee Kafka publish even on crash |
| **Circuit Breaker** | Stop calling a failing service | Dependency that fails consistently |
| **Bulkhead** | Isolate thread pools per downstream | One slow service starving others |
| **Idempotency** | Same op multiple times = same result | Any retry-able operation |
| **CQRS** | Separate read/write models | High R:W ratio with different access patterns |
| **Event Sourcing** | State = sequence of events | Audit / time-travel queries needed |
| **Blue-Green** | Two live envs, cut traffic over | Major releases needing instant rollback |
| **Canary** | Deploy to small % first | Gradual rollout with real validation |
| **Partitioning** | Split data within one DB instance | Query performance on large tables |
| **Sharding** | Split data across multiple DB instances | Horizontal scale beyond single instance |
| **Reactive Streams** | Demand-based flow control | Backpressure in async pipelines |
| **Strangler** | Incremental replacement via facade | Legacy modernization without big-bang rewrite |

---

# APPENDIX B — CAP, ACID, BASE QUICK REFERENCE

**CAP Theorem** — Distributed system can guarantee at most 2 of 3:
- **Consistency** — all nodes see same data
- **Availability** — system always responds
- **Partition tolerance** — system survives network partitions

**Real-world choice:** P is non-negotiable in distributed systems. So you choose between **C and A**.
- **CP systems:** HBase, MongoDB (configurable), traditional RDBMS clusters
- **AP systems:** Cassandra, DynamoDB

**ACID** (relational): Atomicity, Consistency, Isolation, Durability

**BASE** (NoSQL/distributed): Basically Available, Soft state, Eventual consistency

---

# APPENDIX C — KEY NUMBERS FOR DESIGN ANSWERS

| Metric / Story | Anchor |
|---|---|
| OpenSearch tenant-routing fix at Cerner | 700ms → 95ms latency on readmission risk queries |
| Redis cache for Oracle reference data | 40% Oracle load reduction |
| Cache eviction fix at Cerner | 40% memory improvement |
| OpenShift → Kubernetes savings | $5M annual at Optum |
| Eligibility/claims throughput | 5M weekly transactions at Optum |
| Microservices migrated | 110+ at Optum |
| Customer scale | 120+ healthcare providers at Cerner |
| Patient records processed | 500M+ across Cerner customers |
| V3 ML model accuracy improvement | ~40% vs rule-based |
| HPA target CPU | 70% (not 80% — needs headroom) |
| Kafka replication factor (production) | 3, with `min.insync.replicas=2` |
| Headcount growth for 10x customer growth | 30–40% (not 1:1) |

---

*File 4 of 8 — System Design, Architecture, Java/Spring Boot & Kafka (merged master)*
