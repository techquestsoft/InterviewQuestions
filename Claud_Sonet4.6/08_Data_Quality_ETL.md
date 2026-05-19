# FILE 8 OF 8 — DATA QUALITY, ETL PIPELINES & DEEP OBSERVABILITY

> **Rule 1:** Data quality has **FOUR levels** (Completeness → Validity → Consistency → Accuracy). Memorize them.
> **Rule 2:** Every quality answer needs **healthcare-specific examples** — patient encounters, EPMI mapping, ICD-10 codes.
> **Rule 3:** When asked about data integrity, lead with **quality gates at every stage**, not just at the end.

**Structure of every answer:** Memory Hook → Core Answer (framework + reasoning) → Example (specific story with numbers, code, or trade-off).

---

## CROSS-FILE INDEX

This file owns: data quality framework, ETL pipeline integrity, observability gap analysis, big data architecture (C360, Hadoop), Bronze-Silver-Gold pattern, statistical anomaly detection.

- General observability stack (New Relic, Splunk), Kubernetes, cloud, databases → File 06
- Data store partitioning/sharding, Kafka schema/event patterns → File 04
- GenAI data pipelines → File 07

---

# SECTION A — DATA QUALITY FOUR-LEVEL FRAMEWORK

---

## Q1: How do you ensure data integrity and quality in a heterogeneous data pipeline?

**Memory Hook:** Completeness → Validity → Consistency → Accuracy + Governance

> **Core Answer**
>
> I think about data quality at **four levels** — each requires different controls.
>
> **Level 1 — Completeness.** Does every record that should exist actually exist? **Count reconciliation at each pipeline stage** — source count vs landing count vs processed count. Any variance stops the pipeline and alerts.
>
> **Level 2 — Validity.** Does each value conform to its rule? This is **deterministic, per-record checking** — date fields are actually dates, code fields match valid code lists, mandatory fields are not null per contract. Schema-level and business-rule checks built into the processing layer. Example: `patient_id IS NOT NULL` by contract, so any row violating that fails validity.
>
> **Level 3 — Consistency.** Are relationships and logic coherent? Two dimensions:
> - **Referential consistency** — every encounter links to a valid patient, every procedure has a valid encounter, no orphaned records
> - **Temporal/logical consistency** — `discharge_date >= admit_date`, lab result dates fall within the encounter window
>
> Both matter, especially in healthcare data.
>
> **Level 4 — Accuracy.** Does the data reflect reality? This is the hardest to automate directly — true accuracy means matching truth, which usually requires reference data validation. As a proxy, I run **statistical distribution checks**: if a field's null rate jumps from 2% to 40%, that's not a validity failure per se (nulls may be allowed by schema) — but it signals a source-side change that likely impacts accuracy downstream.
>
> **Validity asks: does this value conform to the rule? Accuracy asks: does the data reflect reality?** That distinction matters.
>
> **Plus Governance** — the operating model around the controls. Every field has a data dictionary entry. Every transformation is documented. Every lineage step is traceable. Every dataset has a **named data steward and a freshness SLA**, so quality isn't just a technical control — it has ownership and accountability.

---

## Q2: 15 heterogeneous sources — how do you ensure integrity?

**Memory Hook:** Canonical Schema → Per-Source Mapping → Quarantine → Reconciliation

> **Core Answer**
>
> Four practices for heterogeneous sources.
>
> **Canonical schema as target format.** Every source maps to canonical at ingestion. Records that do not conform are **quarantined, not processed**. Prevents bad data from contaminating downstream layers.
>
> **Per-source mapping layer.** Excel files need a different parser than a REST API, but both produce the same canonical output. **Mapping logic is versioned and tested independently per source.**
>
> **Quarantine zone.** Failed records go to quarantine with error codes. Operations reviews daily. They do not contaminate the main pipeline.
>
> **Reconciliation reports.** Count reconciliation between source and canonical. **Any variance triggers investigation before processing continues.**

---

## Q3: Healthcare data quality — concrete code examples

**Memory Hook:** Identity → Mapping → EPMI → Temporal

> **Core Answer**
>
> Four healthcare-specific validation patterns I've implemented.
>
> **Patient identity validation**
> ```python
> def validate_patient_identity(record):
>     if not record.mrn or not re.match(r'^MRN\d{8}$', record.mrn):
>         flag("INVALID_MRN_FORMAT", record.patient_id)
>     if record.dob > today:
>         flag("DOB_IN_FUTURE", record.patient_id)
>     if record.age > 150:
>         flag("UNREALISTIC_AGE", record.patient_id)
>     if record.gender not in ['M', 'F', 'U', 'O']:
>         flag("INVALID_GENDER_CODE", record.patient_id)
> ```
>
> **Oncology mapping quality**
> ```python
> def validate_oncology_mappings(record):
>     for diagnosis in record.diagnoses:
>         if not is_valid_icd10(diagnosis.code):
>             flag("INVALID_ICD10_CODE", diagnosis.code)
>         if is_cancer_diagnosis(diagnosis.code) and not record.staging:
>             flag("MISSING_CANCER_STAGING", record.patient_id)
>
>     mapped = len([d for d in record.diagnoses if d.oncology_term])
>     coverage = mapped / len(record.diagnoses)
>     if coverage < 0.95:
>         alert(f"Low oncology mapping coverage: {coverage:.1%}")
>         # Expect 95%+ — below this indicates pipeline degradation
> ```
>
> **EPMI association quality**
> ```python
> def validate_epmi_association(record):
>     if not record.epmi_id:
>         flag("EPMI_UNLINKED", record)          # requires manual review
>     if record.dob != epmi_record.dob:
>         flag("EPMI_DOB_MISMATCH", record)      # possible wrong link
>     if count_patients_with_epmi(record.epmi_id) > 1:
>         flag("EPMI_DUPLICATE_LINK", record)    # two patients to one EPMI
> ```
>
> **Temporal consistency (healthcare-specific)**
> ```python
> def validate_temporal_consistency(record):
>     for encounter in record.encounters:
>         if encounter.discharge_date < encounter.admission_date:
>             flag("DISCHARGE_BEFORE_ADMISSION", encounter.id)
>         for procedure in encounter.procedures:
>             if not (encounter.admission_date
>                     <= procedure.date
>                     <= encounter.discharge_date):
>                 flag("PROCEDURE_OUTSIDE_ENCOUNTER_WINDOW", procedure.id)
> ```
>
> **Why these specifically**
>
> These are the four classes of healthcare data error I've actually seen produce wrong clinical decisions: bad identity (treatment for the wrong patient), bad mapping (wrong risk score), bad EPMI (duplicated or merged patients), bad timeline (procedures before admission). Each is a real failure mode, not a theoretical one.

---

# SECTION B — BRONZE-SILVER-GOLD PATTERN

---

## Q4: Bronze-Silver-Gold data architecture

**Memory Hook:** Bronze (raw) → Silver (cleaned + validated) → Gold (business-ready)

> **Core Answer — Architecture**
>
> ```
> Source Systems (Millennium Oracle, EHR feeds, Lab systems, etc.)
>     │
>     ▼
> [INGESTION: count reconciliation, PK uniqueness, checksum]
>     │
>     ▼
> BRONZE LAYER (Raw)
>     Append-only, source format preserved
>     Used for: replay, audit, reprocessing
>     │
>     ▼
> [TRANSFORMATION: schema enforcement, validation rules,
>                  EPMI association, oncology mapping]
>     │
>     ▼
> SILVER LAYER (Cleaned + Validated)
>     Canonical schema, quality checks passed
>     Used for: downstream processing
>     │
>     ▼
> [ENRICHMENT: business rules, derivations, aggregations]
>     │
>     ▼
> GOLD LAYER (Business-Ready)
>     Application-ready data
>     Used for: APIs, dashboards, ML features
> ```
>
> **Why this pattern works**
>
> - **Bronze** allows replay if business rules change later
> - **Silver** enforces quality without business logic coupling
> - **Gold** serves application needs without re-running quality
> - **Failures at any stage are visible and isolatable**
>
> **Example (Cerner readmission prevention pipeline)**
>
> - **Bronze:** raw patient encounters from Millennium Oracle
> - **Silver:** encounters validated and EPMI-associated
> - **Gold:** risk scores computed, ready for care manager queries
>
> When we upgraded from V1/V2 rule-based to **V3 ML model**, **only the silver-to-gold transformation changed**. Bronze and silver layers were unaffected. Migration was clean — that is the architecture paying off.

---

# SECTION C — QUALITY GATES AT EACH PIPELINE STAGE

---

## Q5: Quality gates per stage — Millennium Oracle to S3 example

**Memory Hook:** Extract → Transform → Load → Post-Load Monitoring

> **Core Answer**
>
> Four checkpoints, each with specific validations.
>
> ```
> Millennium Oracle (Source of Truth)
>       │
>       ▼
> [EXTRACTION CHECKPOINT]
>   ✓ Row count reconciliation (source vs raw layer)
>   ✓ Primary key null and duplicate check
>   ✓ Date gap detection (no missed extraction windows)
>   ✓ Checksum validation (bytes match after transit)
>       │
>       ▼
> [TRANSFORMATION CHECKPOINT]
>   ✓ Record survival count (pre vs post transformation)
>   ✓ EPMI association validation
>   ✓ Oncology mapping coverage (>95%)
>   ✓ Temporal consistency (admission < discharge, etc.)
>   ✓ Referential integrity (encounter → patient, procedure → encounter)
>       │
>       ▼
> [LOAD CHECKPOINT]
>   ✓ S3 write verification (count + checksum)
>   ✓ Deduplication via idempotent upsert
>   ✓ PHI completeness for required fields
>       │
>       ▼
> [POST-LOAD MONITORING]
>   ✓ Statistical volume anomalies (vs 30-day rolling avg)
>   ✓ Null rate drift detection
>   ✓ Mapping coverage KPI trends
>   ✓ Pipeline end-to-end latency vs SLO
> ```
>
> **Discipline rule**
>
> Gates are **non-negotiable** — if any checkpoint fails, the pipeline stops and alerts. Bad data not progressing is far cheaper than bad data reaching the gold layer and corrupting downstream reports.

---

## Q6: Statistical anomaly detection — how it catches real issues

**Memory Hook:** Volume Anomaly + Null Drift + Coverage Trend

> **Core Answer**
>
> **Row-level checks catch obvious failures. Statistical checks catch systemic issues row-level checks miss.**
>
> ```python
> def run_statistical_checks(daily_batch):
>     avg_daily_volume = get_30day_rolling_average()
>
>     if daily_batch.count < avg_daily_volume * 0.7:
>         alert("VOLUME_DROP: possible extraction failure or Millennium issue")
>     if daily_batch.count > avg_daily_volume * 1.5:
>         alert("VOLUME_SPIKE: possible duplicate extraction")
>
>     null_rate = calculate_null_rate(daily_batch, field="oncology_term")
>     if null_rate > historical_null_rate + 0.05:
>         alert("NULL_RATE_INCREASED: mapping pipeline degradation?")
> ```
>
> **Example (real incident at Cerner)**
>
> The **10x metadata spike** (Care Coordination, 120-client platform) was **first detected by volume anomaly, not by job failure**. The job did not fail — it *"succeeded"* but processed 10x more data than expected. **Statistical detection caught it ~3 hours before SLA breach.**
>
> Without volume anomaly detection, we would have learned about it from a client SLA breach alert. With it, we caught it early enough to prevent customer impact.

---

## Q7: Idempotency in data pipelines

**Memory Hook:** Same Operation, Same Result

> **Core Answer**
>
> ```python
> def upsert_patient_record(record):
>     existing = fetch_from_s3(patient_id=record.patient_id)
>     if existing:
>         if existing.checksum == record.checksum:
>             skip("NO_CHANGE_DETECTED")
>         else:
>             update_with_version(record)       # changed — version bump
>     else:
>         insert_new(record)
>
> # S3 key design for idempotency
> s3_key = f"patients/{patient_id}/record.json"        # Overwrite-safe
> # NOT: f"patients/{patient_id}/{timestamp}.json"     # Creates duplicates
> ```
>
> **Why this matters**
>
> **Retries are inevitable in pipelines.** Without idempotency, a retry doubles or duplicates data. With idempotency, retries are safe — same operation produces same result.
>
> **Example (real incident)**
>
> The **47-minute Kafka replay incident** (readmission prevention pipeline, three health systems impacted) was caused by **missing idempotency on a downstream consumer**. Replaying Kafka from the last committed offset reprocessed messages that had already been processed once.
>
> **Fix:** idempotency check using a **deduplication key with 24-hour TTL**. Same incident has not recurred in six months.

---

# SECTION D — METRICS & MONITORING FOR DATA PIPELINES

---

## Q8: KPIs to track for data pipeline health

**Memory Hook:** Volume → Drop Rate → Coverage → Latency

> **Core Answer**
>
> | Metric | Target / Alert Threshold |
> |--------|--------------------------|
> | Extraction row count vs source | **Any mismatch** — alert immediately |
> | Record drop rate in transformation | Alert if **> 1%** |
> | EPMI unlinked rate | Alert if **> 0.5%** |
> | Oncology mapping coverage | Alert if **< 95%** |
> | Daily volume vs 30-day average | Alert if **< 70% or > 150%** |
> | S3 write success rate | Alert if **< 100%** |
> | Pipeline end-to-end latency | Alert if **> defined SLO** |
> | Null rate for critical fields | Alert if **drift > 5% from baseline** |
>
> **Discipline rule**
>
> Every metric on this list **must have an alert and an owner**. Metrics without alerts become dashboards no one reads. Metrics without owners become tickets no one fixes.

---

## Q9: How do you debug when a downstream report shows wrong data?

**Memory Hook:** Lineage Trace → Specific Record → Specific Transformation → Root Cause

> **Core Answer**
>
> **Use lineage.** Every record carries provenance — which source, which version of the mapping, which pipeline run. If a downstream report shows an anomaly, I can trace back to the exact source record and transformation that produced it.
>
> **Investigation flow**
>
> 1. Identify the wrong value in the report
> 2. Find the gold-layer record that produced it
> 3. Trace to the silver-layer record
> 4. Trace to the bronze-layer record
> 5. Compare against source system
>
> **Each step has a quality check. The first stage where the value diverges from source is the failure point.**
>
> **Example (Cerner)**
>
> A downstream **readmission risk report showed unexpected nulls for a specific health system**. Lineage trace showed the data was **correct in bronze, correct in silver, but null in gold**. Root cause: a recent change to the gold-layer aggregation had an incorrect join. **Caught and fixed within 2 hours instead of days.**

---

# SECTION E — BIG DATA ARCHITECTURE (FROM YOUR EXPERIENCE)

---

## Q10: C360 Big Data Platform at Optum (reference architecture)

**Memory Hook:** Spark + Kafka + Cassandra + Hive — Tens of Millions of Records

> **Core Answer**
>
> I architected and led development of **C360 — a big data platform unifying tens of millions of consumer records into a single view**.
>
> **Components**
>
> - **Ingestion:** Kafka for real-time events from source systems (eligibility, claims, benefits, member portal)
> - **Processing:** Spark for batch ETL and feature engineering
> - **Storage:** Cassandra for low-latency lookup (consumer 360 view); Hive on HDFS for analytical workloads
> - **Serving:** REST APIs over Cassandra for product applications; Hive queries for analytics teams
>
> **Why this stack**
>
> - **Cassandra** — write-heavy workload with predictable read patterns by consumer ID. Partition key = consumer ID, sub-second lookup
> - **Hive** — complex analytical queries that didn't need real-time
> - **Spark** — unified batch and streaming framework; same code worked for daily batch and near-real-time updates
> - **Kafka** — decoupled producers from consumers, allowed reprocessing
>
> **Outcome**
>
> Unified consumer view feeding **fraud detection, personalization, and member experience** use cases. **Replaced multiple inconsistent point-to-point integrations.**

---

## Q11: Hadoop Fraud Analytics Platform at Bank of America

**Memory Hook:** Real-Time Decision + Batch Analytics → 20% Fraud Reduction, ~$10M Saved

> **Core Answer**
>
> Led development of a **Hadoop-based fraud analytics platform** processing large-scale transactional data.
>
> **Architecture**
>
> - **HDFS** for transaction history at scale
> - **MapReduce** for batch fraud pattern analysis (later moved to Spark)
> - **Hive** for analyst queries on historical patterns
> - **Real-time decision engine** consuming the patterns for live transaction scoring
>
> **Outcome**
>
> - **20% reduction in fraud loss**
> - **Approximately $10M annual savings**
>
> **Earlier, separately, I led a real-time decision engine** that reduced credit loss by **20–40%**. This was about scoring credit decisions in real time at the point of transaction — different system, different team, but **same general pattern: real-time scoring informed by batch-trained models.**

---

## Q12: Real-time vs batch processing — when to use which

**Memory Hook:** Latency Requirement Drives the Choice

> **Core Answer**
>
> | Need | Latency | Approach |
> |------|---------|----------|
> | Live transaction scoring | **< 100ms** | Real-time — Kafka + Spark Streaming or Flink |
> | Fraud pattern detection (long-term) | Hours to days | Batch — Spark on HDFS |
> | Risk score recalculation (clinical) | < 6 hours | **Hybrid** — incremental batch every few hours |
> | Daily reporting | Daily | Batch — Spark + Hive |
> | ML model training | Daily/weekly | Batch — Spark MLlib |
>
> **Example (Cerner readmission risk scoring)**
>
> **Hybrid** — base scores recalculated daily via batch (Spark), but updates triggered by significant patient events (admission, discharge) are processed in **near-real-time** (Kafka stream). **Latency target: 6 hours from event to updated score** available to care manager.

---

# SECTION F — DEEP OBSERVABILITY FOR DATA PIPELINES

---

## Q13: Pipeline observability — beyond standard APM

**Memory Hook:** Logs + Metrics + Traces + Events + Data Quality Metrics

> **Core Answer**
>
> For data pipelines, the standard four pillars (logs / metrics / traces / events) need a **fifth dimension: data quality metrics as time series.**
>
> ```
> Standard observability:
>   - Logs: pipeline execution logs in Splunk
>   - Metrics: job duration, success rate in New Relic
>   - Traces: distributed traces across pipeline stages
>   - Events: deployment markers, config changes
>
> Plus for data:
>   - Data quality metrics as time series:
>       * Daily volume per source
>       * Null rate per critical field
>       * Mapping coverage rates
>       * EPMI unlinked rate
>       * Quarantine zone size
> ```
>
> **These metrics, charted over time, surface degradation before customer impact.**
>
> **Example**
>
> A 30-day chart of EPMI unlinked rate that ticks from 0.3% to 0.8% over two weeks is a slow-burn quality issue — invisible to any single job's success/failure check, but obvious in a time-series view.

---

## Q14: Reactive vs proactive monitoring shift

**Memory Hook:** Job Failure Alerts (reactive) → Anomaly Detection (proactive)

> **Core Answer**
>
> **Key learning from the metadata spike incident at Cerner.**
>
> **Before the incident:** monitoring was reactive — relied on **job failure alerts**, investigated after repeated failures to reduce noise. **We were detecting issues only after impact had begun.**
>
> **Post-incident shift to proactive:**
>
> - **Data volume anomaly detection** (statistical baseline + threshold)
> - **Guardrails to prevent abnormal reprocessing** (validation at metadata update layer)
> - **Early SLA warning signals** (alert at 70% of SLA window, not when breached)
>
> **Outcome**
>
> Now we **detect and stop issues before customer impact, not after.** This is the architectural difference between mature and immature pipeline operations.

---

# QUICK REFERENCE — MEMORY HOOKS

| # | Topic | Memory Hook |
|---|---|---|
| Q1 | Data quality framework | Completeness → Validity → Consistency → Accuracy + Governance |
| Q2 | Heterogeneous sources | Canonical Schema → Per-Source Mapping → Quarantine → Reconciliation |
| Q3 | Healthcare quality examples | Identity → Mapping → EPMI → Temporal |
| Q4 | Bronze-Silver-Gold | Raw → Cleaned → Business-Ready |
| Q5 | Quality gates per stage | Extract → Transform → Load → Post-Load Monitoring |
| Q6 | Statistical anomaly detection | Volume Anomaly + Null Drift + Coverage Trend |
| Q7 | Idempotency | Same Operation, Same Result |
| Q8 | Pipeline KPIs | Volume → Drop Rate → Coverage → Latency |
| Q9 | Debug wrong data | Lineage Trace → First Divergence = Failure Point |
| Q10 | C360 (Optum) | Spark + Kafka + Cassandra + Hive |
| Q11 | Hadoop Fraud (BoA) | Real-Time + Batch → 20% Reduction, ~$10M Saved |
| Q12 | Real-time vs batch | Latency Requirement Drives the Choice |
| Q13 | Deep observability | 4 Pillars + Data Quality Time Series |
| Q14 | Reactive → Proactive | Detect Before Impact, Not After |

---

# APPENDIX A — REAL DATA STACKS (PORTFOLIO REFERENCE)

| Project | Stack | Outcome |
|---------|-------|---------|
| **C360 (Optum)** | Spark, Kafka, Cassandra, Hive, HDFS | Unified consumer 360 view from tens of millions of records |
| **Hadoop Fraud Analytics (BoA)** | HDFS, MapReduce/Spark, Hive | 20% fraud reduction, ~$10M annual savings |
| **Real-Time Decision Engine (BoA)** | Real-time scoring + batch-trained models | 20–40% credit loss reduction |
| **V3 Risk Scoring (Cerner)** | Java/Spark POC, OpenSearch, ML model | ~40% accuracy improvement vs rule-based |
| **Readmission Pipeline (Cerner)** | Metadata-driven, Kafka, OpenSearch | 120-client SLAs maintained >95% |

---

# APPENDIX B — KEY NUMBERS FOR DATA ANSWERS

| Metric / Anchor | Value |
|---|---|
| Oncology mapping coverage threshold | ≥ 95% |
| EPMI unlinked alert threshold | > 0.5% |
| Record drop rate alert threshold | > 1% |
| Volume anomaly alert thresholds | < 70% or > 150% of 30-day average |
| Null rate drift alert | > 5% from baseline |
| Statistical detection lead time (10x spike incident) | ~3 hours before SLA breach |
| 47-min Kafka replay incident | Three health systems, idempotency missing |
| Idempotency dedup key TTL | 24 hours |
| Cerner Care Coordination clients | 120 |
| Cerner V3 ML model improvement | ~40% accuracy vs rule-based |
| Cerner readmission scoring latency target | 6 hours event → score |
| BoA fraud reduction | 20% (~$10M annual) |
| BoA credit loss reduction | 20–40% (real-time decision engine) |
| Optum C360 platform scale | Tens of millions of consumer records |

---

*File 8 of 8 — Data Quality, ETL Pipelines & Deep Observability (merged master)*
