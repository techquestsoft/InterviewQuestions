# Wells Fargo — SEM Interview Prep
**Role:** Software Engineering Manager — Risk & Finance Data Platform
**Created:** June 2026 | Refresh doc — sits alongside Files 01–08

> **Tonight's rule:** Don't re-read all 8 files. Focus only on Parts 2, 3, 5, 6 (the gaps). Say each answer out loud at least once.

---

## Part 0 — What Wells Fargo Is Actually Looking For

This role is NOT a pure product engineering manager. Three things make it distinct:

- **SRE mindset** — reliability, availability, recoverability are first-class concerns (not afterthoughts)
- **Regulated data platform** — Risk, Finance, Regulatory reporting. Auditability + reconciliation under strict deadlines
- **Data engineering leadership** — Apache Iceberg, Hive, Spark, Kafka are desired tech. Expected to understand these deeply

> ⚠️ **Your existing files cover ~75% of this interview.** Four gaps need new answers: Apache Iceberg, SRE reframing, Regulatory data deadlines, and Why Wells Fargo. All below.

---

## Part 1 — Refresh Status by Topic

| Topic | Status | Action |
|---|---|---|
| Introduction & Career Story (File 01) | ✅ Ready | Swap last 2 sentences — see Part 9 |
| Why Wells Fargo | ⚠️ Must prepare | Full answer in Part 2 |
| People & Behavioral (File 02) | ✅ Ready | Disengaged Engineer + Conflict stories — use as-is |
| Delivery & KPIs (File 03) | 🔄 Light refresh | Emphasize "strict deadline" + regulatory = fixed dates |
| System Design & Kafka (File 04) | ✅ Ready | Lead with streaming architecture |
| Production Reliability (File 06) | 🔄 Reframe | Use SRE ownership language — Part 5 |
| Data Quality & ETL (File 08) | 🔄 Reframe | Add Iceberg + reconciliation/audit framing — Parts 3 & 6 |
| Apache Iceberg | 🆕 New | Part 3 |
| NexusOne / Private Cloud | 🆕 New framing | Part 7 |
| SRE Principles | 🆕 New framing | Part 5 |
| Regulatory Data Platforms | 🆕 New framing | Part 6 |

---

## Part 2 — Why Wells Fargo (NEW — Must Memorize)

**Memory Hook:** Domain Return → Regulated Scale → Data Platform Fit

### Core Answer

Three reasons.

**First — Domain return at the highest scale.**
I spent 11 years at Bank of America, where I led a real-time credit card decision engine that reduced credit loss by 20–40%, and a Hadoop-based fraud analytics platform that saved approximately $10 million annually. Banking is where I built my technical and engineering management foundations. Wells Fargo's scale and the criticality of Risk and Finance data is exactly the kind of environment where my experience translates directly.

**Second — This role matches how I think about data platforms.**
The JD describes Risk and Finance data platforms with accountability for reliability, data correctness, and regulatory reporting under strict deadlines. That is not just an engineering problem — it is an engineering + operational + governance problem simultaneously. At Optum and Cerner, I owned platforms where correctness was non-negotiable: incorrect data in clinical systems means wrong patient care decisions. Risk and Finance data has the same stakes — incorrect data means wrong regulatory filings, wrong capital calculations. I am comfortable with that level of accountability.

**Third — The stack is exactly where I operate.**
Spark, Kafka, Hive, and the Apache Iceberg ecosystem are technologies I have built and operated at scale — C360 at Optum, readmission pipelines at Cerner, the Hadoop fraud platform at BoA. The CI/CD, observability, and incident management practices listed align with how my teams operate today.

**Close:**
> *"What I find most compelling about this role is that it combines SRE-level reliability ownership with data engineering leadership in a regulated environment. Most roles ask for one or the other. I have done both in healthcare, and I am ready to bring that to financial services."*

> ⚠️ Do NOT use the JPMC/BBAO framing from File 01 Section E. Replace completely with the above.

---

## Part 3 — Apache Iceberg (NEW — WF Desired Qualification)

> 🆕 Apache Iceberg is explicitly listed in the JD. Not covered in any of your 8 files.

**Memory Hook:** Table Format → ACID on Data Lakes → Time Travel → Schema Evolution → Partition Evolution

### Q: What is Apache Iceberg and why does it matter for Risk/Finance platforms?

**Core Answer — What It Is**

Apache Iceberg is an open table format for large-scale analytic datasets on data lakes (S3, HDFS, OCI Object Storage). It solves three problems that traditional Hive tables could not:
- ACID transactions on data lakes
- Reliable schema evolution without breaking downstream consumers
- Time-travel queries for historical data access

| Capability | Why It Matters for Risk & Finance |
|---|---|
| **ACID Transactions** | Multiple writers can safely update the same table — critical when multiple Risk pipelines land data concurrently |
| **Time Travel** | Query data as-of any point in time — essential for regulatory reporting ("show me risk exposure as of quarter-end") |
| **Schema Evolution** | Add columns without breaking existing readers — finance schemas evolve with regulatory requirements |
| **Partition Evolution** | Change partitioning strategy without rewriting all data — critical for large historical datasets |
| **Snapshot Isolation** | Readers see consistent snapshots even while writers are updating — no dirty reads |
| **Incremental Processing** | Efficiently process only changed data since last checkpoint — critical for large daily Risk data volumes |

**Iceberg vs Traditional Hive Tables**

- **Hive:** partition-based, no ACID (append-mostly), schema changes are dangerous, no time travel
- **Iceberg:** file-level tracking via manifest files, full ACID, safe schema/partition evolution, built-in time travel via snapshots

**How I Would Use Iceberg for a Risk/Finance Platform**

- **Bronze layer (raw):** append-only Iceberg tables — immutable, time-travel capable for audit
- **Silver layer (validated):** Iceberg with merge-on-read for corrections and reconciliation updates
- **Gold layer (reporting):** Iceberg partitioned by reporting date — snapshot isolation ensures quarter-end data is frozen even if post-period corrections arrive
- **Regulatory point-in-time:** time travel query reconstructs exactly what the data looked like at regulatory cutoff — no manual archiving needed

> ⚠️ Iceberg competes with Delta Lake (Databricks) and Apache Hudi. WF listed Iceberg specifically — don't pivot to Delta Lake.

### Q: How do you handle late-arriving data corrections in Iceberg?

Iceberg's merge-on-read pattern — write corrections as new files, merge at read time. For financial reconciliation, I maintain a corrections table alongside the base table, with a reconciliation job that produces the official view. Correction records are immutable once written — complete audit trail. The Iceberg snapshot for any reporting date is locked and queryable even after corrections are applied.

---

## Part 4 — Big Data Stack: Spark + Kafka + Hive (Reframe)

> Your File 08 covers C360 and Hadoop fraud platform. Reframe these for Risk/Finance context.

### Q: Walk me through a Risk data pipeline end-to-end

**Memory Hook:** Ingestion (Kafka) → Bronze (Iceberg) → Validation → Silver → Reconciliation → Gold (Reporting)

**Core Answer — Five layers:**

1. **Ingestion:** Kafka for real-time events from source systems (trade feeds, market data, position updates). Kafka provides replay capability — critical if downstream processing fails near a reporting deadline.

2. **Bronze (Raw):** Apache Iceberg tables on object storage. Append-only, immutable. Source format preserved. Time-travel enables audit of exactly what was received and when.

3. **Silver (Validated + Reconciled):** Spark jobs validate completeness (count reconciliation vs source), apply business rules, flag exceptions to a quarantine table. Reconciliation reports generated here — every record reconciled back to source before proceeding.

4. **Gold (Reporting-Ready):** Iceberg tables partitioned by reporting date. Snapshot isolation guarantees the quarter-end view is frozen even as subsequent corrections arrive.

5. **Auditability:** Complete data lineage from source system through every transformation to the regulatory report. Any auditor can trace any number in any report back to the source record and transformation logic.

### Q: How do you handle Spark job failures near a regulatory deadline?

**Memory Hook:** Checkpoint → Retry → Prioritize SLA → Communicate Early

- **Spark checkpointing enabled** — failed jobs restart from last checkpoint, not from scratch
- **Job prioritization by deadline** — YARN queue or Spark dynamic allocation configured to prioritize regulatory deadline jobs
- **Early alert at 70% of SLA window** — if a regulatory report is due at 8 AM and the pipeline hasn't completed by 5 AM, escalate immediately. Don't wait for 7:55 AM.
- **Playbook per regulatory report** — documented runbook: what to do if primary pipeline fails, what manual overrides exist, escalation path

---

## Part 5 — SRE Principles (Reframe Your Existing Answers)

> The JD says: *"Apply SRE principles: Observability, automation and tooling."* Your File 06 is strong — reframe using SRE language.

### Q: How do you apply SRE principles to a data platform?

**Memory Hook:** Reliability Ownership → SLOs (4 dimensions) → Error Budgets → Toil Reduction → Blameless Culture

**Core Answer — Three levels:**

**Level 1 — Define what reliability means for data platforms (beyond uptime).**

For a Risk/Finance data platform, reliability has four dimensions:
- **Availability** — is the pipeline running?
- **Data Correctness** — is the output reconciled to source?
- **Timeliness** — did the data land by the SLA deadline?
- **Recoverability** — if something fails, how fast can we get back to a known-good state?

I define SLOs across all four — not just uptime.

**Level 2 — Reduce toil through automation.**

- Manual reconciliation → automated count reconciliation at each pipeline stage with automated alerting
- Manual incident response steps → runbooks codified as automated remediation scripts where possible
- Manual release gates → automated quality gates in CI/CD so humans approve outcomes, not execute steps
- Alert fatigue from noisy alerts → every alert must be actionable. Alerts that fire and self-resolve get deleted or re-thresholded.

**Level 3 — Error budget mindset for platform changes.**

When reliability is below target, freeze non-critical changes and focus on reliability improvement. When reliability is strong, invest more aggressively in new capabilities. This creates a natural forcing function — teams cannot ship endless new features while accumulating reliability debt.

| SRE Concept | How I Apply It |
|---|---|
| SLOs for data platforms | Availability + Data Correctness + Timeliness + Recoverability — all four tracked |
| Error budgets | When SLO <99.5%, freeze non-critical changes; focus on reliability |
| Toil reduction | Automate reconciliation checks, incident runbooks, release gates |
| Blameless postmortems | Every P1/P2 → RCA within 48 hours, CAPA with owners and dates |
| Observability | Four pillars: logs (Splunk) + metrics (New Relic) + traces + events (deployment markers) |
| Proactive monitoring | Alert at 70% SLA window remaining; statistical anomaly detection for volume spikes |

---

## Part 6 — Regulatory Data Platforms (NEW Framing)

> 🆕 The JD mentions Risk, Finance, and Regulatory reporting under strict deadlines. Adapt your data quality answers for this context.

**Memory Hook:** Fixed Deadline → Auditability → Reconciliation → Data Lineage → Escalation Protocol

### Q: How do you manage a data platform where the deadline is regulatory and cannot slip?

**Core Answer — Three operating modes:**

**Steady-State (daily operations):**
- Pipeline SLAs expressed as countdown to regulatory cutoff, not just job duration. Track all upstream pipelines against their must-complete-by times.
- Daily reconciliation reports showing source vs processed counts. Any variance investigated before the next pipeline run — never carry forward unresolved variance.
- Complete data lineage for every dataset feeding a regulatory report — traceable from source system to report field, including transformation logic and version.

**Early Warning (when a pipeline is at risk):**
- Alert at 70% of the SLA window — not at breach. Gives time to activate contingencies.
- Defined escalation tree: pipeline engineer → data platform lead → EM → business stakeholder. Each level has a response time SLA.
- Manual fallback procedures documented for critical reports.

**Crisis (when deadline is in jeopardy):**
- Incident commander model — one person owns the resolution, not a committee.
- Parallel tracks: technical team diagnoses and resolves; EM communicates on 15-minute cadence to stakeholders.
- Regulatory escalation decision — if a report cannot be filed accurately, the *business* makes the call. Engineering provides accurate data on pipeline status and options — not the regulatory decision.

### Q: How do you ensure data is reconciled and auditable for regulatory purposes?

**Memory Hook:** Count Reconciliation → Lineage Trace → Immutable Bronze → Signed-Off Gold

- **Count reconciliation at every stage** — source count, landing count, validation count, output count. Any break triggers investigation before processing continues.
- **Immutable raw (bronze) layer** — source data never overwritten, only appended. Complete audit history from day one.
- **Data lineage metadata** — every record carries source system ID, ingestion timestamp, pipeline version, transformation rules applied.
- **Gold layer snapshot locking** — regulatory report datasets locked at cutoff. Subsequent corrections applied to a corrections layer, not the locked snapshot.
- **Reconciliation sign-off gate** — before any data is used in a regulatory report, a reconciliation report must be generated and signed off by the data steward. No automated bypass.

---

## Part 7 — Private Cloud & NexusOne Intelligence

> NexusOne Intelligence is Wells Fargo's internal data platform. You don't need deep knowledge — just a credible framing.

### Q: Have you worked with private cloud or hybrid cloud environments?

**Memory Hook:** Private Cloud at Optum → On-Prem Kubernetes → OCI Migration at Cerner → Hybrid Patterns

- At Optum, the Data Private Cloud was on-premise infrastructure managed by the platform team. My teams ran workloads on it — Kubernetes clusters, Hadoop clusters, Kafka brokers — without direct cloud provider access. The operating model is similar to what WF describes: dedicated infrastructure, internal platform teams managing the substrate, application teams consuming services. The key skill is working effectively with the internal platform team — understanding their roadmap, their change processes, and influencing their priorities.
- At Cerner, we operated in a hybrid model — AWS for existing services, OCI for new deployments, on-prem for some compliance-sensitive data. Managing across environments requires strong IaC discipline (Terraform) and environment-agnostic application design (12-factor).

> ⚠️ If asked specifically about NexusOne: *"I am not familiar with NexusOne's specific internals — but the operating model of an internal intelligence platform serving multiple data consumers is familiar from my Optum and Cerner experience. I would invest the first 30 days understanding their architecture, APIs, and roadmap before drawing any conclusions."*

---

## Part 8 — Questions to Ask Wells Fargo

Pick 2. Don't ask more unless they invite more.

1. What does the data platform landscape look like today — are teams migrating to Apache Iceberg, or is Hive still the primary table format for most Risk datasets?
2. How are regulatory reporting deadlines managed operationally — is there a dedicated SRE function, or does the engineering team own end-to-end reliability including deadline adherence?
3. What does the relationship between this team and the Risk/Finance business stakeholders look like — how much of the roadmap is driven by regulatory requirements versus platform capability investments?
4. Where is the team today on observability maturity for data pipelines — beyond infrastructure monitoring, are data quality metrics tracked as time series?

---

## Part 9 — Introduction Swap for Wells Fargo

Your standard intro ends with a healthcare/JPMC close. Swap only the last 2 sentences.

**Replace:**
> *"What draws me to this role is the scale, the healthcare impact, and the opportunity..."*

**With:**
> *"What draws me to this role is the return to financial services at the highest scale — Risk and Finance data platforms where data correctness is not a nice-to-have but a regulatory requirement. My 11 years at Bank of America gave me the domain foundation. My subsequent platform engineering experience at Optum and Cerner gave me the data engineering and SRE depth. I am ready to bring both together in this role."*

> ⚠️ Everything else in the 90-second intro stays the same.

---

## Part 10 — Numbers to Have Ready (Resume-Consistent)

| Number | Context |
|---|---|
| 20+ years total experience | Intro and career walkthrough |
| 11 years at Bank of America | Domain return framing — banking credibility for WF |
| $10M annual savings | Hadoop fraud analytics at BoA — most relevant to WF Risk platform |
| 20% fraud reduction | BoA Hadoop platform outcome |
| 20–40% credit loss reduction | BoA real-time decision engine |
| $5M annual savings | Optum OpenShift → Kubernetes — engineering efficiency story |
| 110+ microservices migrated | Optum Kubernetes migration scale |
| 120+ customers | Cerner Care Coordination — SLA accountability at scale |
| 14 engineers | Team size at Cerner |
| ~40% accuracy improvement | V3 ML risk scoring at Cerner |
| >95% SLO maintained | Cerner production reliability |
| 47 minutes MTTR | Kafka replay incident — specific incident story |
| P1 MTTD target | Under 5 minutes |
| P1 MTTR target | Under 30 minutes |
| Change Failure Rate target | Below 5% |
| 20% sprint capacity for tech debt | Delivery discipline answer |

---

## Part 11 — Tonight's Rehearsal Checklist

Say these out loud. Timed.

- [ ] 90-second intro with Wells Fargo close (Part 9) — time it
- [ ] Why Wells Fargo (Part 2) — 2 minutes max
- [ ] Apache Iceberg explanation (Part 3) — 90 seconds, no jargon
- [ ] Risk data pipeline architecture (Part 4) — draw on paper first, then say it
- [ ] SRE principles for data platforms (Part 5) — use the four reliability dimensions
- [ ] How you handle regulatory deadlines (Part 6)
- [ ] Production incident story — 47-minute Kafka replay (File 06 Q3)
- [ ] One behavioral STAR story — disengaged senior engineer (File 02 Q7)
- [ ] 3 executive KPIs — Set A: MTTD + MTTR + CFR (File 03 Q18)
- [ ] Data quality four-level framework — Completeness → Validity → Consistency → Accuracy (File 08 Q1)

> ⚠️ Focus ONLY on Parts 2, 3, 5, 6 tonight. Everything else is already solid in your existing files.

---

*Companion to Files 01–08 | Wells Fargo SEM Interview*
