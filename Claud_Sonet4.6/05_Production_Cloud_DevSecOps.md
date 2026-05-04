# Interview Prep — File 5 of 7
# Production Reliability, Cloud, DevSecOps & Observability

> **Rule 1:** Production questions — lead with management response (blast radius, communication, mitigation), THEN technical root cause.
> **Rule 2:** Cloud questions — be specific about which services and WHY. Generic answers score 5/10.
> **Rule 3:** Observability — four pillars (logs, metrics, traces, events). Most candidates miss events.

---

## CROSS-FILE INDEX

This file owns: production incidents, observability, AWS/Azure/OCI services, Kubernetes, CI/CD, DevSecOps, security.
- KPIs (MTTD/MTTR/CFR) → File 03
- Data quality and pipeline integrity → File 07

---

## SECTION A — PRODUCTION RELIABILITY

---

### Q1: What is your overall approach to production reliability?

**Memory Hook:** Observe → Resilience → Improve → SLO Buffer

> "Three pillars, plus an operating discipline.
>
> **Observability** — you cannot fix what you cannot see. Metrics, logs, distributed traces, and event markers. All four. Without events, you cannot answer the most important incident question: what changed just before this started?
>
> **Resilience** — design assuming failures will happen. Retries with exponential backoff, circuit breakers, fallbacks, bulkheads. Every external dependency is a potential failure point.
>
> **Continuous improvement** — every incident produces a structural fix, not just a workaround. RCA within 48 hours. CAPA items with owners and dates.
>
> **Operating discipline.** I define SLOs per service and set alerts at 99.5% when the SLO target is 99.9%. That gives me time to investigate before the SLO is breached, not after. Alerts must be actionable — if an alert fires and the response is 'nothing to do, it self-resolved,' the threshold needs adjustment or the alert needs deletion. Recurring incidents without a structural fix indicate a broken CAPA process — that gets escalated.
>
> My goal: move from incident handling to incident prevention."

---

### Q2: How do you handle a production incident?

**Memory Hook:** Detect → Blast Radius → Mitigate → Communicate Cadence → RCA → CAPA

> "Structured SEV framework.
>
> **First 5 minutes.** Declare severity, assign incident commander — one person owns the incident, not a committee. Open a war-room channel. Assess blast radius — how many customers affected, what is the business impact?
>
> **First 15 minutes.** Communicate to stakeholders with what we KNOW, not what we think. 'Three clients impacted since 14:32. Root cause under investigation. Next update in 15 minutes.' Maintain that cadence throughout.
>
> **Mitigation before root cause.** Rollback the deployment, flip the feature flag, failover to backup. Restore service first. Customers cannot wait for RCA.
>
> **Post-incident.** Blameless postmortem within 48 hours. Five whys. Action items specific, owned, time-bound. Not 'add more monitoring' — specific: 'Add p99 alert on inventory Kafka consumer by Friday, owner: [name].'
>
> **Real example.** Our readmission prevention pipeline had a data processing failure affecting three health systems. I led the incident. Mitigated in 47 minutes by replaying Kafka from last committed offset. Postmortem surfaced a missing idempotency check — fixed within the sprint. Zero recurrence for six months."

---

### Q3: Tell me about a real production incident you owned

#### Incident A — External Dependency Failures (Faraday auth service)

**Memory Hook:** Intermittent 500s → Faraday → Retries + Backoff + Fallback

**Situation:** Routine monitoring showed intermittent HTTP 500 spikes across services. System was largely self-recovering but spikes risked SLA impact.

**Action:**
1. **Containment.** Confirmed failures were intermittent and recovering, but posed reliability risk.
2. **Root cause.** Traced to external authorization dependency (Faraday service) with transient network issues.
3. **Resilience improvements.** Retries with exponential backoff, timeouts and fallback handling, graceful degradation.
4. **Observability enhancements.** Alerts for 500-error spikes, dependency-level failure tracking, dashboards distinguishing external vs internal issues.

**Result:** Significantly reduced impact of transient failures. Improved system resilience and external-dependency visibility.

**Key learning:** Even when issues are transient and self-recovering, proactively strengthening retries, fallbacks, and observability is critical.

#### Incident B — Metadata Spike (10x Volume Reprocessing)

**Memory Hook:** 10x Spike → Cleanup Script → API-controlled Updates + Anomaly Detection

**Situation:** In our readmission prevention platform serving 120 clients with metadata-driven incremental pipelines, a sudden 10x spike in data volume caused multiple job failures and one client SLA breach.

**Action (3 phases):**
1. **Customer impact mitigation.** Identified affected clients, isolated impacted pipelines. Prioritized recovery for SLA-critical workloads. Kept stakeholders informed throughout.
2. **System stabilization.** Paused pipelines to prevent overload. Discovered incorrect metadata was causing reprocessing of historical data. Corrected metadata and restarted jobs in phased, controlled manner.
3. **Root cause and prevention.** Cleanup script directly updated shared metadata table without validation. Governance gap exposed.

**Structural fix:**
- Eliminated direct database writes to shared metadata
- API-based controlled updates with validation rules (e.g., reject backward timestamp changes)
- Approval workflows for high-risk changes
- Scoped updates to reduce blast radius
- Anomaly detection alerting for abnormal data volumes

**Result:** System stability restored within hours. Strengthened with governance, validation, and proactive monitoring.

**Key learning (observability):** Monitoring was reactive — relied on job failure alerts. Post-incident, shifted to proactive — data volume anomaly detection, guardrails against abnormal reprocessing, early SLA warning signals. Now we detect and stop issues before customer impact.

#### Incident C — HBase to OpenSearch Migration (Data visibility loss)

**Memory Hook:** Migration Code Defect → Failed Index Creation → Lost Fallback

**One-line summary:** "Upstream cluster failure during a migration window was not communicated to downstream consumers, causing one client to lose data visibility for 4 hours."

**One-line prevention:** "We introduced a post-migration validation gate and a downstream notification protocol for upstream changes."

**Additional answer when pushed on "how do you enforce notification if upstream doesn't know who their consumers are?":**
> "This is a service dependency registry problem. Every service that consumes another service's API registers itself as a dependent. When upstream plans a change or maintenance, the registry automatically notifies registered consumers. Removes manual memory dependency. We proposed this as a platform-level initiative after the incident."

---

### Q4: How do you ensure logging is adequate, especially for legacy systems?

**Memory Hook:** Transaction Sampling → Throughput Correlation → Error Coverage

**Critical:** Ananth at Availity asked this exact question. The answer he wanted was specific measurement methods, not generic "we standardize logging."

> "Two scenarios — new system and legacy system.
>
> **New system.** Logging requirements are user stories, not afterthoughts. We define a log contract per API — what is logged at entry, exit, and on error. Validate in staging by replaying known transactions and verifying expected log entries appear.
>
> **Legacy / inherited system — log coverage audit:**
>
> **Method 1 — Transaction sampling.** Take 100 known transaction IDs from the source database for a specific time window. Search Splunk for those IDs. If a transaction ID exists in the database but produces no log entries, you have a logging gap on that code path.
>
> **Method 2 — Throughput correlation.** Database record count for a time window should match log event count within tolerance. If database shows 100K records processed and Splunk shows 60K events, you have a 40% logging gap.
>
> **Method 3 — Error classification coverage.** Every 5xx in the API layer should have a corresponding error log with specific code and context. If you see 500 spikes in New Relic but cannot find corresponding logs, the error path is not logging.
>
> Prioritize fixing error path logging gaps first — those are most dangerous for incident diagnosis."

---

## SECTION B — OBSERVABILITY

---

### Q5: How do you design for observability?

**Memory Hook:** Logs + Metrics + Traces + Events = Complete Observability

#### The four pillars

| Pillar | Answers | Tool in your stack |
|--------|---------|-------------------|
| Logs | What happened, with context | Splunk |
| Metrics | How system is behaving over time | New Relic |
| Traces | Why a specific request was slow | New Relic APM |
| Events | What changed just before the problem | New Relic deployment markers |

**Events are the most commonly missed pillar.**
```
Without events:
  Alert: p99 spike at 14:32
  Logs: slow DB queries
  Metrics: DB CPU jumped at 14:31
  → 2-hour war room trying to find cause

With deployment event at 14:30:
  → Root cause found in 8 minutes
```

#### Three levels of observability

- **Business** — business outcomes (patients processed, orders picked, SLAs met)
- **Service** — p50/p95/p99 latency, error rate, consumer lag
- **Infrastructure** — pod CPU/memory, DB connection pool, GC pause time

---

### Q6: Distributed tracing setup (New Relic + Splunk)

> "Setup is zero code change — just add the agent to Kubernetes deployment.
>
> ```yaml
> env:
>   - name: NEW_RELIC_LICENSE_KEY
>     valueFrom:
>       secretKeyRef:
>         name: newrelic-secret
>         key: license-key
>   - name: NEW_RELIC_APP_NAME
>     value: "orders-service"
>   - name: NEW_RELIC_DISTRIBUTED_TRACING_ENABLED
>     value: "true"
> command: ["java", "-javaagent:/newrelic/newrelic.jar", "-jar", "app.jar"]
> ```
>
> New Relic auto-traces HTTP calls, DB queries, Kafka publish/consume across services.
>
> Link traces to Splunk logs via traceId:
> ```xml
> <pattern>traceId=%X{trace.id} spanId=%X{span.id} - %msg%n</pattern>
> ```
>
> In Splunk: `index=prod traceId="abc123" | sort _time` — full log story across all services for one specific request.
>
> **Debugging flow:**
> 1. New Relic alert fires
> 2. Open distributed trace → find slow/failed span → copy traceId
> 3. Splunk: search traceId → full log story across all services
> 4. AKS: kubectl logs / kubectl describe pod if infrastructure-level"

#### When Jaeger?
> "Not needed if New Relic is in the stack. New Relic already provides distributed tracing. Jaeger would be duplication. Only consider Jaeger for: open-source mandate, on-premises trace storage requirement, or no APM license."

---

### Q7: How do you handle support, on-call, and recurring issues?

**Memory Hook:** Own → Prioritize → Analyze → Fix

> "Five practices.
>
> **Ownership** — define clear on-call responsibilities. Rotation across India and US for 24x7 coverage.
>
> **Prioritization** — handle critical issues immediately. Severity-based response times.
>
> **Tracking** — monitor incidents and service requests in a single system.
>
> **Analysis** — identify recurring issues through trend analysis. Same root cause appearing 3+ times = pattern, not coincidence.
>
> **Fix** — implement permanent solutions. Move from reactive fixes to proactive improvements.
>
> Real example at Cerner: we had recurring memory leaks across services because each team was implementing caching independently. Moved to a shared Redis cache with TTL standards. Memory-related incidents dropped 80%."

---

## SECTION C — CI/CD & DEVSECOPS

---

### Q8: Walk me through your CI/CD pipeline end-to-end

**Memory Hook:** Quality → Security → Build → Deploy → Govern

#### Five stages

**Stage 1 — Code Quality**
- PR: 2-level review (peer + manager approval mandatory)
- SonarQube: code coverage gate (90% minimum), maintainability index, tech debt tracking
- PR fails gates → cannot merge

**Stage 2 — Security (Shift-Left)**
- SAST: Fortify / Checkmarx / Veracode / Prisma Cloud (Twistlock)
- Dependency scanning: Snyk / OWASP Dependency Check
- Critical/High CVEs → pipeline blocked, release stopped

**Stage 3 — Build & Packaging**
- Maven / Gradle builds
- Docker image creation
- Artifact stored in Nexus / Artifactory

**Stage 4 — Deployment Pipeline**
- Spinnaker (primary) / Jenkins / GitHub Actions
- Multi-stage: Dev → QA → Pre-Prod → Prod
- Each stage has human approval gate (document approval + executive approval)
- Strategies: Rolling, Canary, Blue-Green

**Stage 5 — Governance & Compliance**
- CAB approval (Change Approval Board) via Remedy CR
- Audit trail: who approved, what changed, when
- RBAC and IAM for environment access
- HIPAA / PCI compliance checks

**Post-Deployment**
- DAST: OWASP ZAP / Burp Suite — runtime security validation
- New Relic monitoring for 24 hours post-release

**Real example:**
> "Fortify identified a high-severity vulnerability during CI. Pipeline automatically blocked the release. Fix applied, revalidated, then CAB approved. Zero production security incidents from that vector."

#### EM-level depth (if probed)
- Policy-as-code (OPA, Kyverno) for automated governance
- DORA metrics tracked (deployment frequency, lead time, MTTR, failure rate)
- Feature flags (LaunchDarkly) for safer releases
- Progressive delivery for risk reduction

---

### Q9: Deployment strategies — when to use which?

| Strategy | How | When | Rollback |
|----------|-----|------|----------|
| **Rolling** | Replace pods gradually (e.g., 2 of 10 at a time) | Routine releases, low risk | Slow — roll forward or backward |
| **Canary** | Deploy to 5–10% of users/clients first | New features, behavior changes | Fast — stop routing to canary |
| **Blue-Green** | Two live environments, cut all traffic at once | Major releases, zero downtime required | Instant — redirect traffic back |

**Real example:**
> "V3 ML model rollout used canary — deployed to 10% of clients, validated prediction accuracy matched baseline, then expanded to 100%. Zero accuracy regression."

#### Enterprise considerations
- Use feature flags (LaunchDarkly / Unleash)
- Monitor KPIs during rollout
- Automated rollback on failure

---

## SECTION D — CLOUD ARCHITECTURE

---

### Q10: AWS Services — design in layers

```
Layer 1 — Edge
    CloudFront (CDN for static content)
    Route 53 (DNS + health checks)
    Front Door equivalent (multi-region routing)

Layer 2 — API & Gateway
    API Gateway (routing, throttling, auth)
    WAF (web application firewall)
    ALB (application load balancer)

Layer 3 — Compute & Orchestration
    EKS (Kubernetes — microservices)
    EC2 + EMR (big data batch pipelines)
    Lambda (event triggers, glue functions)

Layer 4 — Data & Storage
    S3 (data lake — raw, processed, audit)
    OpenSearch (search, analytics, real-time queries)
    RDS / Aurora (transactional)
    DynamoDB (NoSQL transactional)
    ElastiCache / Redis (caching layer)

Layer 5 — Security
    IAM (role-based, least privilege)
    KMS (encryption at rest)
    Secrets Manager (credentials)
    VPC + Security Groups (network isolation)

Layer 6 — Observability
    CloudWatch (infrastructure metrics)
    OpenTelemetry → New Relic (APM, distributed tracing)
    Splunk (log aggregation)
```

#### Real-world cost optimization (EMR)
> "For EMR workloads at Cerner: 60% reserved + 40% spot/transient EC2 instances. The 60% provides cluster baseline (always running), 40% scales with data load. Significantly reduced infrastructure cost while handling burst capacity."

---

### Q11: Azure Services — also relevant for your background

**You hold Azure Solutions Architect Expert certification.** When asked about Azure:

| Need | Azure Service |
|------|--------------|
| Container orchestration | AKS (Azure Kubernetes Service) |
| Serverless | Azure Functions |
| Object storage | Azure Blob Storage |
| Search | Azure Cognitive Search |
| Relational DB | Azure SQL / Azure Database for PostgreSQL |
| NoSQL | Cosmos DB |
| Caching | Azure Cache for Redis |
| Messaging | Service Bus / Event Hub / Event Grid |
| API management | Azure API Management |
| CDN | Azure Front Door |
| Identity | Microsoft Entra ID (formerly Azure AD) |
| Monitoring | Azure Monitor + Application Insights |
| Secrets | Azure Key Vault |
| Multi-region routing | Azure Traffic Manager / Front Door |

---

### Q12: OCI Services — for Cerner migration context

**Currently leading AWS → OCI migration.** Be ready to speak about:
- OCI Compute (equivalent to EC2)
- OCI Object Storage (equivalent to S3)
- OCI Container Engine for Kubernetes (OKE — equivalent to EKS)
- OCI Functions (serverless)
- Oracle Autonomous Database (transactional and analytics)
- OCI API Gateway, OCI Vault (secrets), OCI Logging

#### Why migrate AWS → OCI? (be ready for this question)
> "Strategic decision at Oracle level — consolidating cloud spend on OCI for healthcare products. Cost benefits at the tenant level, deeper integration with Oracle databases (which Cerner products heavily use), and unified support model. The migration approach is service-by-service, with parallel running periods to validate parity before cutover."

---
### Q12a: AWS vs Azure vs OCI

# ☁️ Multi-Cloud Architecture (AWS vs Azure vs OCI)

## 📊 Layered Architecture Mapping

| **Layer** | **AWS** | **Azure** | **OCI (Oracle Cloud)** |
|-----------|--------|----------|------------------------|
| **Layer 1 — Edge** | CloudFront *(CDN for static content — JS, images, SPA assets, caching at edge)*<br>Route 53 *(DNS + health checks — failover, latency routing)* | Azure Front Door *(CDN + global routing — edge caching + L7 routing)*<br>Azure Traffic Manager *(DNS routing — geo/latency-based failover)* | OCI CDN *(static content delivery — edge caching)*<br>OCI DNS *(domain resolution + health checks)* |
| **Layer 2 — API & Gateway** | API Gateway *(routing, throttling, auth — expose REST/GraphQL APIs)*<br>AWS WAF *(web application firewall — block OWASP threats)*<br>ALB *(L7 load balancer — route to microservices)* | Azure API Management *(API gateway — policies, auth, rate limiting)*<br>Azure WAF *(security — SQL injection, XSS protection)*<br>Application Gateway *(L7 routing + load balancing)* | OCI API Gateway *(API exposure — auth, throttling)*<br>OCI WAF *(web protection — attack filtering)*<br>OCI Load Balancer *(traffic distribution — L4/L7)* |
| **Layer 3 — Compute & Orchestration** | EKS *(Kubernetes — microservices orchestration, scaling)*<br>EC2 + EMR *(VMs + big data — Spark/Hadoop batch pipelines)*<br>Lambda *(event triggers — S3 upload → process file, glue logic between services)* | AKS *(Kubernetes — containerized microservices)*<br>VMs + Azure Databricks *(Spark analytics — ETL pipelines)*<br>Azure Functions *(event-driven compute — queue triggers, HTTP triggers)* | OKE *(Kubernetes — container orchestration)*<br>OCI Compute + Data Flow *(Spark jobs — batch processing)*<br>OCI Functions *(serverless — event-driven workflows)* |
| **Layer 4 — Data & Storage** | S3 *(data lake — raw, processed, audit zones)*<br>OpenSearch *(search + analytics — logs, real-time dashboards)*<br>RDS / Aurora *(relational — OLTP transactions)*<br>DynamoDB *(NoSQL — high-scale key-value, sessions)*<br>ElastiCache (Redis) *(caching — reduce DB load, improve latency)* | Blob Storage *(data lake — hierarchical storage, analytics)*<br>Azure Cognitive Search *(search + indexing — app search use cases)*<br>Azure SQL / PostgreSQL *(relational — transactions)*<br>Cosmos DB *(NoSQL — global distribution, low latency)*<br>Azure Cache for Redis *(caching — session store, hot data)* | OCI Object Storage *(data lake — durable storage)*<br>OCI Search Service *(OpenSearch-based — log analytics)*<br>Autonomous Database *(relational — self-tuning OLTP/OLAP)*<br>OCI NoSQL *(key-value store — scalable workloads)*<br>OCI Cache (Redis) *(in-memory caching — performance boost)* |
| **Layer 5 — Security** | IAM *(role-based access — least privilege policies)*<br>KMS *(encryption keys — data at rest protection)*<br>Secrets Manager *(credentials — DB passwords, API keys)*<br>VPC + Security Groups *(network isolation — private subnets)* | Microsoft Entra ID *(identity + RBAC — SSO, enterprise auth)*<br>Azure Key Vault *(keys + secrets — encryption + credentials)*<br>Managed Identities *(secure service auth — no hardcoded secrets)*<br>VNet + NSG *(network isolation — traffic control)* | OCI IAM *(identity + policies — access control)*<br>OCI Vault *(keys + secrets — encryption management)*<br>Compartments *(resource isolation — governance boundaries)*<br>VCN + Security Lists *(network isolation — subnet security)* |
| **Layer 6 — Observability** | CloudWatch *(metrics + logs — CPU, memory, alarms)*<br>X-Ray *(distributed tracing — request flow across services)*<br>OpenTelemetry → New Relic *(APM — latency, bottlenecks)*<br>Splunk *(log aggregation — centralized logging, search)* | Azure Monitor *(metrics + alerts — infra visibility)*<br>Application Insights *(APM — request tracing, failures)*<br>OpenTelemetry → New Relic *(advanced APM)*<br>Splunk *(centralized logging — enterprise observability)* | OCI Monitoring *(metrics — resource health)*<br>OCI Logging *(log aggregation — audit + app logs)*<br>OCI APM *(performance monitoring — tracing)*<br>Splunk *(external log analytics — optional integration)* |
---

### Q12b: Lessons learned from cloud-to-cloud migration (AWS → OCI)

**Memory Hook:** IAC From Day One → Per-Team Scripts Are Tech Debt → Observability Differences

This came up at TJX. Memorize this answer with the specific Saffer example.

> "The biggest lesson: invest in infrastructure-as-code from day one, even if you never plan to migrate.
>
> At Cerner, the Saffer health product (acquired by Oracle four years ago) didn't have Terraform — they had per-team scripts and some manual provisioning. That meant migration from AWS to OCI was significantly more effort than it should have been. Every EMR cluster, every networking rule, every IAM role had to be discovered, documented, and rebuilt — rather than re-applied from version-controlled IaC.
>
> The lesson I now apply: even if you never plan to migrate, infrastructure-as-code is essential for reproducibility, audit, and disaster recovery. Migration becomes a near-side-effect benefit.
>
> Three specific lessons from this AWS → OCI work:
>
> **One — Application code itself migrates cleanly** if it's cloud-agnostic (12-factor, config via environment, no hardcoded service endpoints). Our Kubernetes services and EMR pipelines moved with mostly configuration changes, not code changes.
>
> **Two — Observability has the biggest gap.** New Relic in AWS vs OCI APM are very different. OCI APM is still evolving and doesn't have the same scope. We negotiated to keep New Relic in production while OCI APM matures, then migrate separately. The lesson: build an observability abstraction layer if you anticipate platform changes — common code that supports multiple APM backends saves significant rework.
>
> **Three — Identity and access management requires rework.** IAM roles, OAuth providers, and service-to-service auth are the most cloud-specific layer. Plan for this explicitly in the migration roadmap."

---

### Q13: EC2 vs Lambda — when to use which?

| Decision Factor | Lambda | EC2 |
|----------------|--------|-----|
| Execution duration | Short (< 15 min) | Long-running (hours, EMR) |
| Invocation pattern | Event-driven, sporadic | Continuous, predictable |
| State | Stateless | Can be stateful |
| Cost at scale | Expensive per invocation at high volume | Better per-unit cost with reserved |
| Infrastructure mgmt | Zero | OS patches, scaling config |

#### Lambda use cases — memorize 3 specific examples (Cubic asked this)

**1. Event-driven triggers**
> "Lambda triggered by S3 object creation — when a raw data file lands in our bronze layer, Lambda kicks off the validation and routing to silver layer."

**2. Glue code between pipeline stages**
> "Between EMR job completion and downstream notification — a Lambda checks job exit status and either advances the pipeline or routes to error handling."

**3. SLA monitoring and alerting**
> "Lambda runs on a schedule (CloudWatch Events) checking SLA breaches across our pipelines. If a job has not completed within its SLA window, Lambda posts to Slack and pages on-call."

#### Real usage at Cerner
- EC2 (EMR): 60% reserved + 40% spot for big data pipelines
- Lambda: SLA monitoring, Slack alerting, event triggers between pipeline stages

---

### Q14: Kubernetes — capabilities and real usage

**Memory Hook:** Deploy → Scale → Network → Secure → Observe

| Capability | How | Why |
|-----------|-----|-----|
| Horizontal scaling | HPA on CPU/memory threshold | Auto-handles traffic spikes |
| Self-healing | Liveness + readiness probes | Unhealthy pods replaced automatically |
| Rolling deployments | Replace pods gradually | Zero downtime releases |
| Network isolation | Services + Ingress + NetworkPolicy | Internal vs external routing control |
| Security | RBAC + pod security policies | Least-privilege access |
| Observability | Prometheus + Grafana + New Relic | CPU, memory, pod health |

#### EKS vs Self-managed Kubernetes

| | Self-managed | EKS (Managed) |
|--|-------------|--------------|
| Control plane | You manage | AWS manages |
| Upgrades | Manual | Easier — AWS handles |
| Operational overhead | High | Low |
| AWS integration | Manual | Native |
| Cost | Lower compute, higher operational | Higher compute, lower operational |

**Decision:** unless you have specific compliance or customization needs, EKS — focus engineering effort on product, not platform.

#### Real example
> "At Optum, started with on-prem Kubernetes (platform team owned the cluster). Migrated 110+ microservices from OpenShift to open-source Kubernetes — $5M annual savings. At Cerner, using EKS in AWS — managed control plane meant the platform team focused on developer experience rather than Kubernetes operations. Now migrating to OKE on OCI."

---

## SECTION E — CORE ENGINEERING CONCEPTS

---

### Q15: Memory Leaks — Detection, Diagnosis, Fix

**Memory Hook:** Detect → Diagnose → Fix → Prevent

#### Detection
- APM tools: Dynatrace, New Relic, AppDynamics, Prometheus + Grafana (JVM metrics)
- Monitor: heap usage trends, GC frequency (Full GC spikes), memory growth over time
- Alert threshold: heap usage growing > X% over 4 hours → page on-call

#### Diagnosis
- Capture heap dumps: `jmap -dump:live,format=b,file=heap.hprof <pid>`
- Analyze with Eclipse MAT or VisualVM
- Identify: retained objects, large collections, improper caching, unclosed streams

#### Common root causes
- Cache without TTL or LRU eviction — grows indefinitely
- Static collections holding references — objects never GC'd
- ThreadLocal misuse — values not removed after request
- Unclosed DB connections or streams — connection pool exhaustion

#### Fix strategies
- Introduce TTL or LRU caching (Redis or Guava Cache)
- Resource cleanup via try-with-resources (Java)
- Optimize object lifecycle — do not hold references longer than needed

#### Preventive controls (enterprise level)
- Coding standards via code reviews
- Memory threshold-based alerts
- Dashboards for JVM health
- Memory profiling in performance testing

#### Real example
> "In a patient data service, heap usage kept growing over 6 hours until OOMKill. Root cause: in-memory cache with no eviction — patient records accumulated indefinitely. Fix: switched to Redis with 24-hour TTL. Memory usage dropped 40% and stabilized."

---

### Q16: Deadlocks

**Memory Hook:** Lock Order → Avoid Nesting → Timeout → Async

#### Causes
- Circular dependency on locks
- Nested locking
- Long-running transactions

#### Prevention
- Maintain consistent lock ordering across all code paths
- Avoid nested locks
- Timeout-based locking (`ReentrantLock.tryLock()`)
- Prefer concurrent utilities: ExecutorService, ReentrantLock, CompletableFuture (async)

#### Enterprise practices
- Minimize shared state
- Prefer event-driven / async architecture
- Use DB isolation levels carefully

#### Detection
- Thread dumps: `jstack <pid>` — look for 'DEADLOCK' section
- APM tools (Dynatrace, AppDynamics) — thread contention visibility

#### Real example
> "In a claims processing system, deadlock occurred due to two threads acquiring DB row locks in different order depending on execution path. Fix: standardized lock acquisition order in query logic, reduced transaction scope, added retry with exponential backoff for lock conflicts."

---

## SECTION F — SECURITY & DEVSECOPS

---

### Q17: How do you approach API security?

**Memory Hook:** WAF → OWASP → SAST/DAST → Governance

#### Layer 1 — Infrastructure
- WAF (Web Application Firewall): blocks SQL injection, XSS, CSRF at API gateway
- DDoS protection: rate limiting, connection throttling at entry

#### Layer 2 — Application (OWASP Top 10)
- Broken Authentication: OAuth2 + JWT, short token expiry, secure storage
- Excessive Data Exposure: return only what consumer needs
- Injection: parameterized queries, never string-concatenated SQL
- Security Misconfiguration: no default credentials, no verbose errors in prod
- Broken Access Control: RBAC enforced at service layer, not just UI

#### Layer 3 — Pipeline
- SAST: Fortify, Checkmarx, Veracode — runs on every PR
- DAST: OWASP ZAP, Burp Suite — against staging before prod promotion
- Critical/High vulnerabilities → release blocked
- Dependency scanning: Snyk for known CVEs in libraries

#### Layer 4 — Governance
- CAB approval for production changes
- Audit trail: who deployed what, when, with what test evidence
- RBAC for environment access — production restricted

**Real OWASP training:** You delivered ~3-hour OWASP training to your team. Mention this if asked about security culture.

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

#### SEV definitions

| Level | Definition | Response |
|-------|-----------|----------|
| SEV1 | All customers impacted / data loss risk | Immediate, VP engaged |
| SEV2 | Multiple customers or SLA breach imminent | Immediate, EM leads |
| SEV3 | Single customer, degraded but functional | Same day |
| SEV4/5 | No customer impact, internal quality issue | Next sprint |

---

*File 5 of 7 — Production Reliability, Cloud, DevSecOps & Observability*
