# Kafka & Event-Driven Architecture
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

---

## Q1: How do you design Kafka-based event-driven architectures?

**Memory Trick:** Topic Design → Schema → Producer/Consumer → Resilience → Observability

- **Topic design first** – Topic per event type or domain (e.g., `account.created`, `kyc.completed`). Naming conventions documented org-wide.
- **Schema-driven** – Avro or Protobuf schemas in a Schema Registry. Schema evolution rules (backward/forward compatible).
- **Producer/consumer responsibilities** – Producers own event quality and ordering guarantees. Consumers handle idempotency and retries.
- **Resilience built in** – DLQs (dead letter queues), retry topics, replay capability, idempotent consumers.
- **Observability** – Lag monitoring, throughput, error rate per topic/consumer group, correlation IDs in headers.

---

## Q2: How do you decide topic design — partitions, replication, retention?

**Memory Trick:** Partitions for Scale → Replication for Durability → Retention for Use Case

- **Partitions** – Set based on expected throughput and consumer parallelism. More partitions = more parallel consumers, but also more overhead. Start with what you need; over-partitioning is hard to undo.
- **Partition key** – Choose carefully. For account events, account ID gives ordering per account but can hot-spot if skewed.
- **Replication factor** – 3 in production (tolerates 1 broker failure with `min.insync.replicas=2`). Non-negotiable for critical topics.
- **Retention** – Aligned with use case. Operational topics: 3-7 days. Audit/replay topics: 30+ days or indefinite (compacted).
- **Compaction** – For topics representing latest state (e.g., customer profile), use log compaction.

---

## Q3: How do you ensure exactly-once or at-least-once delivery?

**Memory Trick:** Idempotent Producer → Transactional Write → Idempotent Consumer

- **Idempotent producers** – `enable.idempotence=true` prevents duplicate writes due to retries within Kafka.
- **Transactions for multi-topic atomicity** – Producer transactions when writing to multiple topics or read-process-write patterns.
- **At-least-once is the realistic default** – Truly end-to-end exactly-once requires consumer cooperation.
- **Idempotent consumers** – Use a deduplication key (event ID) and check before processing. State store (DB or Kafka Streams) tracks processed IDs.
- **Design consumers to handle duplicates** – Operations should be safe to repeat (UPSERT, not blind INSERT).

---

## Q4: How do you handle errors and dead letter queues in Kafka consumers?

**Memory Trick:** Retry → Backoff → DLQ → Replay → Alert

- **Categorize errors** – Transient (network, downstream timeout) vs permanent (bad payload, schema violation).
- **Retry with backoff** – For transient errors. Local retry first, then retry topic with delay if needed.
- **Dead letter queue** – Permanent failures go to a DLQ with full context (original payload, error, timestamp, headers).
- **Replay capability** – Tooling to replay DLQ messages after fix is deployed. Don't lose data; recover it.
- **Alert on DLQ growth** – DLQ messages > threshold triggers an alert. DLQ is not a graveyard; it's a triage queue.

---

## Q5: How do you monitor Kafka producers, consumers, and pipelines?

**Memory Trick:** Lag → Throughput → Errors → Health → Alert

- **Consumer lag** – The most important metric. Lag growth means consumer can't keep up — leads to data freshness issues.
- **Throughput** – Messages per second per topic, per consumer group. Sudden drops indicate problems.
- **Error rate** – Failed messages, deserialization errors, retries, DLQ writes.
- **Broker and cluster health** – Under-replicated partitions, ISR shrinks, disk usage.
- **Alerts on SLO breach** – Lag > X minutes, error rate > Y%, throughput drop > Z%. Actionable, not noisy.

---

## Q6: How do you handle schema evolution in Kafka?

**Memory Trick:** Registry → Compatibility Rules → Version → Deprecate

- **Schema Registry** – Confluent Schema Registry or equivalent. All schemas versioned and validated.
- **Compatibility rules** – BACKWARD compatibility by default (new consumers can read old data). FULL compatibility for shared/critical topics.
- **Additive changes only** – Add optional fields, never remove or rename existing ones. Default values for new fields.
- **Version explicitly** – When a breaking change is unavoidable, create a new topic version (`account.created.v2`).
- **Deprecate gracefully** – Notify consumers, run in parallel, monitor v1 usage, retire only when truly unused.

---

## Q7: How do you scale Kafka consumers under load?

**Memory Trick:** Partitions → Consumer Groups → Parallelism → Backpressure

- **Partitions set the upper limit** – Max parallel consumers in a group = number of partitions. Plan partitions for peak load.
- **Consumer groups for scaling out** – Add consumers to share the load. Kafka rebalances automatically.
- **Parallelism within consumer** – If processing per message is heavy, use a thread pool inside the consumer (carefully, to preserve ordering where needed).
- **Backpressure handling** – If downstream is slow, slow consumption (manual commit) rather than dropping messages.
- **Watch for hot partitions** – If one partition gets disproportionate load, revisit partition key strategy.

---

## Q8: How do you build and maintain real-time streaming pipelines?

**Memory Trick:** Source → Transform → Sink → Monitor → Replay

- **Source events** – Kafka topics, with clear ownership and schema.
- **Transform** – Kafka Streams or external processors (Flink for complex stateful processing). Stateless preferred unless state is essential.
- **Sink** – Output to downstream Kafka topic, database, search index, or analytics store. Each sink with its own delivery guarantee.
- **Monitor end-to-end** – Lag from source to sink, transform errors, sink write failures.
- **Replay capability** – Ability to reprocess from offset is a first-class requirement, not an afterthought.

---

## Q9: How do you secure Kafka in a production environment?

**Memory Trick:** TLS → Auth → ACL → Audit → Encrypt PII

- **TLS for all traffic** – Client-broker and inter-broker.
- **Authentication** – SASL (OAuth, SCRAM, or mTLS) for clients. No anonymous access.
- **Authorization (ACLs)** – Topic-level read/write ACLs per service. Principle of least privilege.
- **Audit logging** – Producer/consumer access logged for compliance.
- **Encrypt sensitive payloads** – PII fields encrypted at the application layer before publishing, not just at the network layer.

---

## Q10: How would you design a Kafka-based event flow for an account origination journey?

**Memory Trick:** Capture → Orchestrate → Notify → Audit

- **Capture event** – `application.submitted` published when customer submits onboarding form.
- **Orchestrate downstream work** – KYC service consumes and triggers identity check; document service consumes and processes uploads; risk service consumes and runs scoring.
- **Aggregate state** – An orchestration service consumes individual completion events (`kyc.completed`, `documents.verified`, `risk.cleared`) and emits `application.approved` or `application.rejected`.
- **Notify and update UI** – Notification service consumes final state event, sends email/SMS; UI subscribes to status updates.
- **Audit trail** – All events retained in compacted/audit topic for compliance, replay, and analytics.

> Strong closing line: *"Event-driven architecture is powerful but unforgiving — schema discipline, observability, and replay capability are non-negotiable for production."*

---
