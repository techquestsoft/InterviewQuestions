# Databases – Oracle, Cassandra & MongoDB
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

---

## Q1: How do you decide between Oracle (relational) and NoSQL (Cassandra/MongoDB)?

**Memory Trick:** Schema → Consistency → Scale → Query Pattern

- **Schema needs** – Strict, structured, with strong relationships → Oracle. Flexible, evolving schemas → MongoDB.
- **Consistency requirements** – Strong ACID transactions (financial, account-of-record data) → Oracle. Eventual consistency acceptable → Cassandra.
- **Scale and throughput** – Massive write throughput, horizontal scale, time-series → Cassandra. Mid-scale, mixed workloads → MongoDB or Oracle.
- **Query patterns** – Complex joins and ad-hoc queries → Oracle. Known query patterns by key → Cassandra. Document-centric queries → MongoDB.
- **Operational maturity** – Oracle is well-understood in banking; NoSQL needs different operational skills. Match the platform to team capability.

---

## Q2: How do you design data access patterns in microservices?

**Memory Trick:** Own Data → No Shared DB → Right Tool → Cache Wisely

- **Each service owns its data** – No cross-service direct DB access. Communication is via APIs or events.
- **No shared databases** – Shared schemas create the worst kind of coupling and slow every team down.
- **Right tool per service** – One service might use Oracle for transactional data, another might use MongoDB for document storage. That's fine.
- **Cache where appropriate** – Redis or in-memory cache for hot, read-heavy data. Always with TTL and invalidation strategy.
- **Read/write separation when scale demands** – Read replicas, CQRS for systems where read and write loads diverge significantly.

---

## Q3: How do you handle Oracle database performance tuning at the team level?

**Memory Trick:** Index → Plan → Monitor → Optimize → Govern

- **Index strategy** – Proper indexes on filter and join columns. Too many indexes hurt write performance — balance is key.
- **Execution plans** – Review query plans for slow queries. Look for full table scans, expensive joins, missing statistics.
- **Monitoring** – AWR/ASH reports, slow query logs, lock contention metrics. APM at the application layer to correlate.
- **Optimization** – Query rewrites, hints used judiciously, partitioning for large tables, bind variables to leverage cached plans.
- **Governance** – DBA reviews for schema changes and major queries. Don't let every developer add indexes ad-hoc.

---

## Q4: How do you model data in Cassandra?

**Memory Trick:** Query First → Partition Wisely → Avoid Joins → Time Bucket

- **Model around queries, not entities** – Cassandra rewards designing tables for specific query patterns.
- **Choose partition key carefully** – Determines data distribution and query performance. Avoid hot partitions and unbounded partitions.
- **No joins** – Denormalize. Duplicate data across tables to support different query patterns.
- **Time-based partitioning** – For time-series data, bucket by day/hour to keep partitions manageable.
- **Consistency tuning** – Tune read/write consistency (LOCAL_QUORUM is common) per use case, not globally.

---

## Q5: How do you model data in MongoDB?

**Memory Trick:** Embed vs Reference → Schema Discipline → Index → Aggregation

- **Embed for one-to-few, reference for one-to-many** – Embed when data is read together; reference when it changes independently or grows large.
- **Schema discipline despite flexibility** – Use schema validation. "Schemaless" doesn't mean undisciplined.
- **Indexes are critical** – Compound indexes for multi-field queries. Watch query patterns and add accordingly.
- **Aggregation pipeline for analytics** – Powerful but can be expensive. Test with realistic data sizes.
- **Watch document size** – 16MB limit. Unbounded arrays in documents are an antipattern.

---

## Q6: How do you handle data migrations safely?

**Memory Trick:** Backward Compat → Dual Write → Validate → Cut Over → Decommission

- **Backward-compatible schema changes** – Add new columns/fields, never drop or rename in a single deploy.
- **Dual-write strategy** – Write to old and new schema/store during migration. Read from old until new is validated.
- **Validation in parallel** – Compare reads from old vs new. Discrepancies surface bugs before cutover.
- **Gradual traffic shift** – Move read traffic to new store gradually (1% → 10% → 50% → 100%) with monitoring.
- **Decommission carefully** – Only after all reads and writes are on new store, monitored for weeks, with rollback rehearsed.

---

## Q7: How do you ensure data consistency across distributed services?

**Memory Trick:** Saga → Outbox → Idempotent → Compensation

- **Saga pattern** – Long-running distributed transactions broken into local transactions with events between them.
- **Outbox pattern for atomic event publishing** – Write to local DB and an outbox table in same transaction; separate process publishes to Kafka. Prevents lost events.
- **Idempotent operations** – Every operation safe to retry. Use unique request IDs, conditional updates, UPSERT semantics.
- **Compensating transactions** – When a step fails, run compensating actions to undo previous steps.
- **Accept eventual consistency where possible** – Strong consistency across services is expensive. Most business processes tolerate eventual.

---

## Q8: How do you handle database connection management in microservices?

**Memory Trick:** Pool → Size → Timeout → Monitor → Leak Detection

- **Use connection pools** – HikariCP (Spring Boot default) or equivalent. Never create connections per request.
- **Right-size pool** – Too small = bottleneck; too large = DB overload. Start with reasonable defaults, tune from monitoring.
- **Configure timeouts** – Connection acquisition, query execution, idle timeout. Prevents one slow query from cascading.
- **Monitor pool health** – Active/idle connections, wait time, exhaustion events. Alert on saturation.
- **Detect leaks early** – HikariCP leak detection threshold. Connection leaks cause sudden outages under load.

---

## Q9: How do you handle backups, disaster recovery, and data retention?

**Memory Trick:** Backup → Test Restore → RTO/RPO → Retention → Compliance

- **Regular automated backups** – Full + incremental, encrypted, stored in separate region/account.
- **Test restores regularly** – Backups are useless if you can't restore. Practice quarterly minimum.
- **Define RTO and RPO** – Recovery Time Objective and Recovery Point Objective per service criticality.
- **Retention policy** – Aligned with business and regulatory requirements (SOX, GDPR, banking regulations).
- **Compliance evidence** – Audit logs of access, retention proof, deletion confirmations for regulators.

---

## Q10: How do you ensure data security and PII protection in databases?

**Memory Trick:** Encrypt → Mask → Access Control → Audit → Tokenize

- **Encryption at rest and in transit** – TDE (Transparent Data Encryption) for databases, TLS for connections.
- **Data masking in non-prod** – Never use production PII in dev/test. Mask, anonymize, or generate synthetic data.
- **Strict access control** – Role-based DB access, principle of least privilege, no shared accounts, MFA for admin access.
- **Audit logging** – Every privileged access and query against sensitive data logged for forensics and compliance.
- **Tokenization for highly sensitive data** – Replace card numbers, SSNs with tokens; map kept in a separate, hardened vault.

> Strong closing line: *"Database choices have long-term consequences. I push for the simplest store that meets the requirements, with strong operational and security discipline."*

---
