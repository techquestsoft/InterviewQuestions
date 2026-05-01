# Interview Prep — File 5 of 6
# Production Reliability, Cloud (AWS/Azure), DevSecOps & Observability

> **Rule:** Production questions — lead with management response (blast radius, communication, mitigation) THEN technical root cause.
> **Rule:** Cloud questions — be specific about which services and WHY you chose them. Generic answers score 5/10.

---

## SECTION A — PRODUCTION RELIABILITY & INCIDENTS

---

### Q1: What is your overall approach to production reliability?

**Memory Hook:** Observe → Resilience → Improve

> "I ensure production reliability through three pillars.
>
> Observability — you cannot fix what you cannot see. Metrics, logs, distributed traces, and event markers. All four. Without events, you cannot answer the most important incident question: what changed just before this started?
>
> Resilience — design assuming failures will happen. Retries with exponential backoff, circuit breakers, fallbacks, bulkheads. Every external dependency is a potential failure point.
>
> Continuous improvement — every incident should produce a structural fix, not just a workaround. RCA within 48 hours. CAPA items with owners and dates.
>
> My goal is to move from incident handling to incident prevention."

---

### Q2: How do you handle a production incident?

**Memory Hook:** Detect → Blast Radius → Mitigate → Communicate → RCA → CAPA

> "I follow a structured SEV framework.
>
> First 5 minutes: declare severity, assign incident commander — one person owns the incident, not a committee. Open a dedicated war room channel. Assess blast radius — how many customers affected, what is the business impact.
>
> First 15 minutes: communicate to stakeholders with what we KNOW, not what we think. 'Three clients impacted since 14:32. Root cause under investigation. Next update in 15 minutes.' I maintain that cadence throughout.
>
> Mitigation before root cause: rollback the deployment, flip the feature flag, failover to backup. Restore service first. Customers cannot wait for root cause analysis.
>
> Post-incident: blameless postmortem within 48 hours. Five whys. Action items specific, owned, time-bound. Not 'add more monitoring' — specific: 'Add p99 alert on inventory Kafka consumer by Friday, owner: [name].'
>
> Real example: Our readmission prevention pipeline had a data processing failure affecting three health systems. I led the incident. Mitigated in 47 minutes by replaying Kafka from the last committed offset. Postmortem surfaced a missing idempotency check — fixed within the sprint. Zero recurrence for six months."

---

### Q3: Tell me about a real production incident you owned

**Incident 1 — HBase to OpenSearch Migration (Data Visibility Loss)**

> "During our platform migration from HBase to OpenSearch, we had a Kafka-driven pipeline migrating client data in parallel with ongoing operations. One client's data migration failed mid-process — OpenSearch index was not created due to a code defect.
>
> The fallback was supposed to read from HBase. But HBase had already been migrated — data was no longer there.
>
> Result: one client had no data visibility for 4 hours. SEV2 — single client, SLA breach approaching.
>
> Action: blast radius confirmed as one client. I declared SEV2, informed stakeholders within 10 minutes. We identified the code defect causing the index creation failure, patched and re-ran the migration for that client. Data restored in 4 hours — within SLA.
>
> CAPA: we introduced a migration validation gate — after each client migration, verify index exists in OpenSearch and record count matches HBase. If not, alert before proceeding to next client. No similar incident since."

**One-line summary:** "Upstream migration failure was not communicated to downstream consumers, causing one client to lose data visibility for 4 hours."

**One-line prevention:** "We introduced a post-migration validation gate and a downstream notification protocol for all upstream changes."

---

**Incident 2 — Metadata Spike (10x Volume Reprocessing)**

> "We encountered a sudden 10x spike in data volume across our readmission prevention pipelines — multiple job failures, one client approaching SLA breach.
>
> Root cause: a cleanup script directly updated a shared metadata table without validation, causing the system to reprocess years of historical data instead of only the incremental window.
>
> Action: paused affected pipelines to stop the overload. Corrected the metadata. Restarted jobs in a phased, controlled manner. Restored SLA-critical client first.
>
> Prevention: eliminated direct database writes to shared metadata tables. Introduced API-based controlled updates with validation rules — backward timestamp changes are rejected. Added anomaly detection for abnormal data volume spikes. Alert fires before SLA is breached, not after."

---

### Q4: How do you ensure logging is adequate — especially for legacy systems?

**Memory Hook:** Transaction Sampling → Throughput Correlation → Error Coverage

> "Two approaches depending on whether the system is new or inherited.
>
> For new systems: logging requirements are user stories, not afterthoughts. We define a log contract per API — what is logged at entry, exit, and on error. We validate this in staging by replaying known transactions and verifying expected log entries appear.
>
> For inherited legacy systems: I run a log coverage audit.
>
> Method 1 — transaction sampling: I take 100 known transaction IDs from the source database for a specific time window. I search Splunk for those IDs. If a transaction ID exists in the database but produces no log entries, I have a logging gap on that code path.
>
> Method 2 — throughput correlation: database record count for a time window should match log event count within a small tolerance. If database shows 100K records processed and Splunk shows 60K events, there is a 40% logging gap.
>
> Method 3 — error classification: every 5xx in the API layer should have a corresponding error log in Splunk with a specific error code. If I see 500 spikes in New Relic but cannot find corresponding log entries, the error path is not logging.
>
> I prioritize fixing error path logging gaps first — those are the most dangerous for incident diagnosis."

---

### Q5: How do you ensure production stability at scale?

**Memory Hook:** Monitor → Prevent → Learn

> "Three practices.
>
> Proactive monitoring: I define SLOs per service and set alerts at 99.5% when the SLO is 99.9%. This gives me time to investigate before the SLO is breached, not after. Alerts must be actionable — if an alert fires and the response is 'nothing to do, it self-resolved,' that alert needs its threshold adjusted or it needs to be deleted.
>
> Preventive work: CVE management, tech upgrades, runbooks for known failure modes — all tracked as backlog items with the same rigor as features.
>
> Feedback loop: every incident is an input to the backlog. Every CAPA item has an owner and a date. Incidents that recur without a structural fix indicate the CAPA process is broken."

---

### Q6: How do you design for observability?

**Memory Hook:** Logs + Metrics + Traces + Events = Complete Observability

**The four pillars:**

| Pillar | What it answers | Tool in your stack |
|--------|----------------|-------------------|
| Logs | What happened, with context | Splunk |
| Metrics | How system is behaving over time | New Relic |
| Traces | Why a specific request was slow | New Relic APM |
| Events | What changed just before the problem | New Relic deployment markers |

**Events are the most commonly missed pillar:**
```
Without events:
  New Relic alert: p99 spike at 14:32
  Logs: slow DB queries
  Metrics: DB CPU jumped at 14:31
  → 2-hour war room trying to find cause

With deployment event at 14:30 on dashboard:
  → Root cause found in 8 minutes
```

**Three levels of observability for any system:**
- Business: business outcomes (patients processed, orders picked, SLA met)
- Service: p50/p95/p99 latency, error rate, consumer lag
- Infrastructure: pod CPU/memory, DB connection pool, GC pause time

**Distributed tracing setup (New Relic + Splunk):**
```yaml
# Zero code change — just add agent to Kubernetes deployment
env:
  - name: NEW_RELIC_DISTRIBUTED_TRACING_ENABLED
    value: "true"
command: ["java", "-javaagent:/newrelic/newrelic.jar", "-jar", "app.jar"]
```

Link traces to Splunk logs via traceId:
```xml
<!-- logback.xml — New Relic injects traceId automatically -->
<pattern>traceId=%X{trace.id} - %msg%n</pattern>
```
Then in Splunk: `index=prod traceId="abc123" | sort _time`

**Debugging flow:**
```
New Relic alert fires
    → Find slow/failed span → copy traceId
    → Splunk: search traceId → full log story across all services
    → AKS: kubectl logs if infra-level
```

---

## SECTION B — CI/CD & DEVSECOPS

---

### Q7: Walk me through your CI/CD pipeline end-to-end

**Memory Hook:** Quality → Security → Build → Deploy → Govern

**5 stages:**

**Stage 1 — Code Quality**
- PR: 2-level review (peer + manager approval mandatory)
- SonarQube: code coverage gate (90% minimum), maintainability index, tech debt tracking
- PR fails gates → cannot merge

**Stage 2 — Security (Shift-Left)**
- SAST: Fortify / Checkmarx — static vulnerability scanning
- Dependency scanning: Snyk / OWASP Dependency Check
- Critical/High CVEs → pipeline blocked, release stopped

**Stage 3 — Build & Packaging**
- Maven/Gradle builds
- Docker image creation
- Artifact stored in Nexus/Artifactory

**Stage 4 — Deployment Pipeline**
- Spinnaker: automated multi-stage pipeline (Dev → QA → Pre-Prod → Prod)
- Each stage has human approval gate — document approval + executive approval
- Supports: Rolling (gradual pod replacement), Canary (5–10% subset), Blue-Green (traffic cutover)

**Stage 5 — Governance & Compliance**
- CAB approval (Change Advisory Board) via Remedy CR
- Audit trail: who approved, what changed, when
- RBAC and IAM for environment access
- HIPAA/PCI compliance checks embedded in pipeline

**Post-Deployment:**
- DAST: OWASP ZAP / Burp Suite — runtime security validation
- New Relic monitoring for 24 hours post-release

**Real example:**
> "At Cerner, Fortify identified a high-severity vulnerability during CI. Pipeline automatically blocked the release. Fix applied, revalidated, then CAB approved. Zero production security incidents from that vector."

---

### Q8: Deployment strategies — when to use which?

| Strategy | How It Works | When to Use | Rollback |
|---------|-------------|------------|---------|
| Rolling | Replace pods gradually (e.g., 2 of 10 at a time) | Routine releases, low risk | Slow — roll forward or backward |
| Canary | Deploy to 5–10% of users/clients first | New features, behavior changes | Fast — stop routing to canary |
| Blue-Green | Two live environments, cut all traffic at once | Major releases, zero downtime required | Instant — redirect traffic back |

**Real example:**
> "For the V3 ML model rollout, we used canary — deployed to 10% of clients, validated prediction accuracy matched baseline, then expanded to 100%. Zero production accuracy regression."

---

## SECTION C — CLOUD ARCHITECTURE

---

### Q9: AWS Services — how do you design in layers?

**My AWS Stack Design Layers:**

```
Layer 1 — Edge
    CloudFront (CDN for static content)
    Route 53 (DNS + health checks)
    Front Door (geo-routing for multi-region)

Layer 2 — API & Gateway
    API Gateway (routing, throttling, authentication)
    WAF (web application firewall)
    ALB (application load balancer)

Layer 3 — Compute & Orchestration
    EKS (Kubernetes — microservices)
    EC2 + EMR (big data batch pipelines)
    Lambda (event triggers, short-lived functions)

Layer 4 — Data & Storage
    S3 (data lake — raw, processed, audit)
    OpenSearch (search, analytics, real-time queries)
    RDS / Aurora (transactional workloads)
    ElastiCache / Redis (caching layer)

Layer 5 — Security
    IAM (role-based access — least privilege)
    KMS (encryption at rest)
    Secrets Manager (credentials management)
    VPC + Security Groups (network isolation)

Layer 6 — Observability
    CloudWatch (infrastructure metrics)
    OpenTelemetry → New Relic (APM, distributed tracing)
    Splunk (log aggregation and search)
```

---

# AWS vs Azure — Layered Architecture Comparison

---

## Layer 1 — Edge

| AWS | Azure |
|-----|------|
| CloudFront (CDN for static content) | Azure CDN / Azure Front Door |
| Route 53 (DNS + health checks) | Azure DNS |
| Front Door (geo-routing for multi-region) | Azure Front Door (global routing + WAF) |

---

## Layer 2 — API & Gateway

| AWS | Azure |
|-----|------|
| API Gateway (routing, throttling, authentication) | Azure API Management |
| WAF (web application firewall) | Azure Web Application Firewall |
| ALB (application load balancer) | Azure Application Gateway |

---

## Layer 3 — Compute & Orchestration

| AWS | Azure |
|-----|------|
| EKS (Kubernetes — microservices) | AKS (Azure Kubernetes Service) |
| EC2 + EMR (big data batch pipelines) | Azure Virtual Machines + Azure Databricks |
| Lambda (event triggers, short-lived functions) | Azure Functions |

---

## Layer 4 — Data & Storage

| AWS | Azure |
|-----|------|
| S3 (data lake — raw, processed, audit) | Azure Blob Storage |
| OpenSearch (search, analytics, real-time queries) | Azure Cognitive Search |
| RDS / Aurora (transactional workloads) | Azure SQL Database |
| ElastiCache / Redis (caching layer) | Azure Cache for Redis |

---

## Layer 5 — Security

| AWS | Azure |
|-----|------|
| IAM (role-based access — least privilege) | Azure Active Directory (Azure AD) |
| KMS (encryption at rest) | Azure Key Vault |
| Secrets Manager (credentials management) | Azure Key Vault |
| VPC + Security Groups (network isolation) | Azure Virtual Network (VNet) + NSG |

---

## Layer 6 — Observability

| AWS | Azure |
|-----|------|
| CloudWatch (infrastructure metrics) | Azure Monitor |
| OpenTelemetry → New Relic (APM, distributed tracing) | Azure Application Insights / OpenTelemetry → New Relic |
| Splunk (log aggregation and search) | Splunk / Azure Monitor Logs |

---

### Q10: EC2 vs Lambda — when to use which?

| Decision Factor | Lambda | EC2 |
|----------------|--------|-----|
| Execution duration | Short (< 15 min) | Long-running (hours, EMR jobs) |
| Invocation pattern | Event-driven, sporadic | Continuous, predictable |
| State | Stateless | Can be stateful |
| Cost at scale | Expensive per invocation at high volume | Better per-unit cost with reserved |
| Infrastructure management | Zero | OS patches, scaling config |

**My real usage at Cerner:**
- EC2 (EMR): 60% reserved + 40% spot instances for big data pipelines. Static cluster for overhead, transient EC2s scale with data load.
- Lambda: SLA monitoring triggers, Slack alerting, event-driven functions between pipeline stages.

---

### Q11: Kubernetes — key capabilities and real usage

**Memory Hook:** Deploy → Scale → Network → Secure → Observe

| Capability | How | Why |
|-----------|-----|-----|
| Horizontal scaling | HPA — scales pods on CPU/memory threshold | Auto-handles traffic spikes |
| Self-healing | Liveness + readiness probes | Unhealthy pods replaced automatically |
| Rolling deployments | Replace pods gradually | Zero downtime releases |
| Network isolation | Services + Ingress + NetworkPolicy | Internal vs external routing control |
| Security | RBAC + pod security policies | Least-privilege access |
| Observability | Prometheus + Grafana + New Relic | CPU, memory, pod health |

**EKS vs self-managed:**
- Self-managed: full control, high overhead, manual upgrades
- EKS: AWS manages control plane, easier upgrades, better AWS service integration
- Decision: unless you have specific compliance or customization needs, EKS — focus engineering effort on product, not platform

**Real example:**
> "At Optum, migrated from OpenShift to self-managed Kubernetes. At Cerner, used EKS — managed control plane meant the platform team could focus on developer experience rather than Kubernetes operations. Migrating now to OCI (Oracle Cloud Infrastructure) Kubernetes for vendor consolidation."

---

### Q12: How do you handle memory leaks in Java services?

**Memory Hook:** Detect → Diagnose → Fix → Prevent

**Detection:**
- New Relic / Dynatrace: heap usage trends, Full GC frequency, memory growth over time
- Alert threshold: if heap usage grows > X% over 4 hours, page on-call

**Diagnosis:**
- Capture heap dump: `jmap -dump:live,format=b,file=heap.hprof <pid>`
- Analyze with Eclipse MAT or VisualVM
- Look for: retained objects, large collections, cache without eviction, unclosed streams

**Common root causes:**
- Cache without TTL or LRU eviction — grows indefinitely
- Static collections holding references — objects never GC'd
- ThreadLocal misuse — values not removed after request
- Unclosed DB connections or streams — connection pool exhaustion

**Fix strategies:**
- Introduce TTL or LRU caching (Redis or Guava Cache)
- Ensure proper resource cleanup (try-with-resources in Java)
- Optimize object lifecycle — do not hold references longer than needed

**Real example:**
> "In a patient data service, heap usage kept growing over 6 hours until OOMKill. Root cause: an in-memory cache with no eviction — patient records accumulated indefinitely. Fix: switched to Redis with 24-hour TTL. Memory usage dropped 40% and stabilized."

---

### Q13: How do you handle deadlocks?

> "Deadlocks occur when two threads hold locks and wait for each other's lock indefinitely.
>
> Prevention:
> - Consistent lock ordering — always acquire locks in the same order across all code paths
> - Avoid nested locks when possible
> - Use timeout-based locking (ReentrantLock with tryLock)
> - Prefer async/event-driven architecture over synchronous shared state
>
> Detection:
> - Thread dumps: `jstack <pid>` — look for 'DEADLOCK' section
> - APM tools (Dynatrace, AppDynamics) — thread contention visibility
>
> Real example: In a claims processing system, a deadlock occurred due to two threads acquiring DB row locks in different order depending on execution path. Fix: standardized lock acquisition order in the query logic, reduced transaction scope, added retry with exponential backoff for lock conflicts."

---

## SECTION D — SECURITY (OWASP & DevSecOps)

---

### Q14: How do you approach API security?

**Memory Hook:** WAF → OWASP → SAST/DAST → Governance

**Layer 1 — Infrastructure:**
- WAF (Web Application Firewall): blocks SQL injection, XSS, CSRF at API gateway level
- DDoS protection: rate limiting, connection throttling at entry

**Layer 2 — Application (OWASP Top 10):**
- Broken Authentication: OAuth2 + JWT, short token expiry, secure storage
- Excessive Data Exposure: return only what the consumer needs — not full entity
- Injection: parameterized queries, never string-concatenated SQL
- Security Misconfiguration: no default credentials, no verbose error messages in production
- Broken Access Control: RBAC enforced at service layer, not just UI

**Layer 3 — Pipeline:**
- SAST (static): Fortify, Checkmarx — runs on every PR
- DAST (runtime): OWASP ZAP against staging before production promotion
- Critical/High vulnerabilities → release blocked
- Dependency scanning: Snyk for known CVEs in libraries

**Layer 4 — Governance:**
- CAB approval for production changes
- Audit trail: who deployed what, when, with what test evidence
- RBAC for environment access — production access is restricted

---

## QUICK REFERENCE — INCIDENT MANAGEMENT

| Phase | Action | Owner |
|-------|--------|-------|
| Detect | Alert fires or user reports | Monitoring system |
| Declare | Assign SEV level and incident commander | EM / on-call lead |
| Communicate | Stakeholder update within 15 min | Incident commander |
| Mitigate | Rollback / failover / isolate | Engineering team |
| Stabilize | Confirm customer impact resolved | EM |
| RCA | 5 whys, timeline reconstruction | Engineering team |
| CAPA | Specific fixes with owners and dates | EM + engineers |
| Close | Blameless postmortem document shared | EM |

**SEV Definitions:**
| Level | Definition | Response |
|-------|-----------|---------|
| SEV1 | All customers impacted / data loss risk | Immediate, VP engaged |
| SEV2 | Multiple customers or SLA breach imminent | Immediate, EM leads |
| SEV3 | Single customer, degraded but functional | Same day |
| SEV4/5 | No customer impact, internal quality issue | Next sprint |

---

*File 5 of 6 — Production Reliability, Cloud, DevSecOps & Observability*
