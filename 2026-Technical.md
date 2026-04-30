# PART 1: Core Engineering Concepts (Foundation)

## Q1: Memory Leaks

I approach memory leaks in three phases:

### 1. Detection
- Use APM tools:
  - Dynatrace
  - New Relic
  - AppDynamics
  - Prometheus + Grafana (JVM metrics)
- Monitor:
  - Heap usage trends
  - GC frequency (Full GC spikes)
  - Memory growth over time

### 2. Diagnosis
- Capture heap dumps
- Analyze using:
  - Eclipse MAT (Memory Analyzer Tool)
  - VisualVM
- Identify:
  - Retained objects
  - Large collections
  - Improper caching

### 3. Fix
Common causes:
- Unclosed DB connections / streams
- Static collections holding references
- Improper cache eviction strategies
- ThreadLocal misuse

Fix strategies:
- Introduce TTL or LRU caching (Redis/Guava)
- Ensure proper resource cleanup (try-with-resources)
- Optimize object lifecycle

### Preventive Controls (Enterprise Level)
- Enforce coding standards via code reviews
- Add memory alerts (threshold-based monitoring)
- Set up dashboards for JVM health
- Include memory profiling in performance testing

### Real Scenario:
In a patient data service, heap usage kept increasing.
Root cause: cache without eviction.

Fix:
- Introduced TTL-based caching
- Added monitoring alerts
- Reduced memory usage by ~40%

---
## Q2: Deadlocks

Deadlocks occur when multiple threads hold locks and wait indefinitely for each other.

### Causes:
- Circular dependency on locks
- Nested locking
- Long-running transactions

### Prevention:
- Maintain consistent lock ordering
- Avoid nested locks
- Use timeout-based locking
- Prefer concurrent utilities:
  - ExecutorService
  - ReentrantLock
  - CompletableFuture (async)

### Enterprise Practices:
- Minimize shared state
- Prefer event-driven / async architecture
- Use DB isolation levels carefully

### Detection:
- Thread dumps
- APM tools (Dynatrace, AppDynamics)

### Real Scenario:
In a claims processing system:
- Deadlock occurred due to DB row locks

Fix:
- Reduced transaction scope
- Introduced retry with exponential backoff
- Optimized query locking strategy

# PART 2: CI/CD & DevSecOps

## Q3: CI/CD Pipeline

### 🧠 Memory Hook
Quality → Security → Build → Deploy → Govern

---

### 🎯 Core Answer

Our CI/CD pipeline is designed with **DevSecOps principles**, ensuring **security, quality, and governance are integrated end-to-end** across the software delivery lifecycle—not treated as separate steps.

It is structured into five key stages:

---

### 1. Code Quality

- PR reviews (2-level approval: peer + manager)
- SonarQube:
  - Code quality
  - Code coverage
  - Maintainability index
  - Code smells & technical debt tracking

👉 Ensures only high-quality, maintainable code progresses

---

### 2. Security (Shift-Left)

- SAST tools:
  - Fortify
  - Checkmarx
  - Veracode
  - Prisma Cloud (Twistlock)

- Dependency scanning:
  - Snyk
  - OWASP Dependency Check

👉 Security is validated early (shift-left), preventing vulnerabilities from reaching later stages

---

### 3. Build & Packaging

- Maven / Gradle builds
- Docker image creation (containerization)
- Artifact storage:
  - Nexus
  - Artifactory

👉 Ensures consistent, reproducible builds across environments

---

### 4. Deployment Pipeline

- Tools:
  - Spinnaker (primary)
  - Jenkins / GitHub Actions (in some setups)

- Environment flow:
  Dev → QA → Pre-prod → Prod

- Supports deployment strategies:
  - Rolling
  - Canary
  - Blue-Green

👉 Enables automated, reliable, and scalable deployments

---

### 5. Governance & Compliance (Enterprise Critical)

- CAB approval (Change Advisory Board)
- Release audit trails (who approved, what changed)
- Environment-level access control (RBAC, IAM)
- Automated rollback strategy
- Compliance checks (HIPAA / PCI depending on domain)

👉 Ensures controlled releases, auditability, and regulatory compliance

---

### 6. Runtime Security (Post-Deployment)

- DAST tools:
  - OWASP ZAP
  - Burp Suite

👉 Continuous security validation even after deployment

---

### ⚖️ Trade-offs

- Faster delivery vs governance control  
- Full automation vs controlled enterprise releases  
- Developer velocity vs compliance requirements  

---

### 🏢 Real Scenario

In our healthcare platform:

- Fortify identified a high-severity vulnerability during CI stage  
- Pipeline automatically blocked the release  
- Fix was applied and revalidated before CAB approval  

Outcome:
- Prevented a potential production security issue  
- Ensured compliance with healthcare regulations  

---

### 🔍 If Probed (EM-Level Depth)

- Use policy-as-code (OPA, Kyverno) for automated governance  
- Track DORA metrics (deployment frequency, lead time, MTTR, failure rate)  
- Integrate feature flags (LaunchDarkly) for safer releases  
- Implement progressive delivery strategies for risk reduction  
---

## Q4: Deployment Strategies

We use multiple deployment strategies based on risk:

### 1. Rolling Deployment
- Gradual replacement of instances
- No downtime
- Example:
  10 pods → replace 2 at a time

### 2. Blue-Green Deployment
- Two environments (Blue = current, Green = new)
- Switch traffic after validation

### 3. Canary Deployment
- Release to small % of users (5–10%)
- Monitor before full rollout

### Enterprise Considerations:
- Use feature flags (LaunchDarkly / Unleash)
- Monitor KPIs during rollout
- Automated rollback on failure

### Real Scenario:
For ML model rollout:
- Deployed to 10% users (canary)
- Validated prediction accuracy
- Then scaled to 100%
---

# PART 3: AWS & Cloud Architecture

## Q5: AWS Services Used

I typically design AWS systems in layers:

### Compute & Orchestration
- EKS (Kubernetes for microservices)
- EC2 (EMR clusters for big data)

### Storage & Data
- S3 (data lake: raw + processed)
- OpenSearch (search & analytics)
- RDS / DynamoDB (transactional workloads)

### Serverless
- Lambda (event-driven processing)

### Networking
- VPC (network isolation)
- API Gateway (routing, throttling)
- CloudFront (CDN)

### Security
- IAM (role-based access)
- KMS (encryption)

### Observability
- CloudWatch
- OpenTelemetry + New Relic

### Real Scenario:
For EMR workloads:
- Used 60% reserved + 40% spot instances
- Reduced infrastructure cost significantly
---

## Q6: EC2 vs Lambda

### Lambda (Serverless)
Use when:
- Event-driven workloads
- Short-lived execution
- No infrastructure management needed

### EC2
Use when:
- Long-running jobs (EMR, batch processing)
- Need OS-level control
- Predictable workload (cost optimization)

### Enterprise Tradeoffs:
- Lambda → easy scaling, but limited execution time
- EC2 → more control, but requires management

### Real Scenario:
- EC2 used for EMR clusters (big data pipelines)
- Lambda used for:
  - SLA monitoring
  - Slack alerting
  - Event triggers
---

## Q7: Kubernetes Usage

We use Kubernetes for container orchestration:

### Deployment & Scaling
- Docker containers
- HPA (Horizontal Pod Autoscaler)

### Networking
- Services (internal communication)
- Ingress (external routing)

### Resilience
- Self-healing (pod restart)
- Rolling deployments

### Observability
- Prometheus + Grafana
- OpenTelemetry
- New Relic

### Security
- RBAC
- Network policies

### Real Scenario:
Migrated 100+ microservices from OpenShift to EKS:
- Improved scalability
- Reduced infra cost (~$5M annually)

## Q8: EKS vs Self-managed Kubernetes

### Self-managed Kubernetes
- Full control
- High operational overhead
- Manual upgrades & patching

### EKS (Managed)
- AWS manages control plane
- Easier upgrades
- Better AWS integration

### Decision Factors:
- Team expertise
- Operational cost
- Scalability needs

### Real Scenario:
Moved to EKS to:
- Reduce platform maintenance
- Improve reliability
- Focus engineering effort on product features
---

