# Cloud Platforms & Containers (AWS, Cloud Foundry, Docker, Kubernetes)
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

---

## Q1: How do you guide your team in deploying services on cloud and container platforms?

**Memory Trick:** Containerize → Orchestrate → Secure → Observe → Scale

- **Containerize consistently** – Standardized base images, multi-stage builds, minimal images for security and size.
- **Orchestrate with Kubernetes** – Deployments, Services, Ingress, ConfigMaps, Secrets. Helm or Kustomize for templating.
- **Secure by default** – RBAC, network policies, pod security policies, image scanning, secret management.
- **Observability built-in** – Metrics (Prometheus), logs (ELK/Splunk), traces (OpenTelemetry/New Relic), correlation IDs.
- **Scale based on signals** – HPA on CPU/memory/custom metrics, VPA for right-sizing, cluster autoscaler.

---

## Q2: How do you decide between Kubernetes, Cloud Foundry, and serverless?

**Memory Trick:** Workload Profile → Team Maturity → Org Standards → Cost

- **Kubernetes** – Complex services, microservices at scale, when you need flexibility and portability. Higher operational overhead.
- **Cloud Foundry** – PaaS abstraction. Faster onboarding for app teams, but less control. Good for standard 12-factor apps.
- **Serverless (Lambda)** – Event-driven, short-lived, sporadic workloads. Great for glue code and async processors. Watch for cold starts and cost at scale.
- **Team maturity** – K8s requires platform expertise. If the team isn't ready, managed services or PaaS reduce risk.
- **Org standards** – Align with what the platform team supports. Going off-paved-path increases ops cost.

---

## Q3: How do you architect microservices on Kubernetes for resilience and scale?

**Memory Trick:** Health → Resources → Autoscale → Disrupt → Network

- **Health checks** – Liveness, readiness, startup probes. Misconfigured probes are a leading cause of outages.
- **Resource requests and limits** – Set both. Without requests, scheduling is arbitrary; without limits, noisy neighbors.
- **Horizontal autoscaling** – HPA on CPU, memory, or custom metrics (e.g., Kafka lag). Min/max sized for cost and burst.
- **Pod disruption budgets** – Prevent too many pods from going down during voluntary disruptions (node drains, upgrades).
- **Network policies** – Default deny, then explicitly allow service-to-service traffic. Reduces blast radius.

---

## Q4: How do you manage configuration and secrets in Kubernetes?

**Memory Trick:** ConfigMap → Secret → External Vault → Rotation

- **ConfigMaps for non-sensitive config** – Versioned, environment-specific, mounted as files or env vars.
- **Secrets for sensitive data** – Base64-encoded by default, but use external vault integration for stronger protection.
- **External secret management** – HashiCorp Vault or AWS Secrets Manager via External Secrets Operator. Single source of truth.
- **Avoid secrets in env vars** – Mount as files when possible; env vars leak into logs and crash dumps.
- **Rotation** – Regular rotation for credentials. Use short-lived tokens where supported.

---

## Q5: How do you handle service-to-service communication and networking in Kubernetes?

**Memory Trick:** Service → Ingress → Service Mesh → Policy

- **Services for in-cluster discovery** – ClusterIP for internal, with stable DNS.
- **Ingress for external traffic** – NGINX, ALB, or cloud-managed ingress. TLS termination, routing, rate limiting.
- **Service mesh (Istio/Linkerd) when complexity justifies** – mTLS, retries, circuit breaking, observability without code changes. Don't add it until you need it.
- **Network policies** – Microsegmentation. Default deny, explicit allow.
- **Egress control** – Restrict outbound traffic to known destinations. Reduces data exfiltration risk.

---

## Q6: How do you optimize cloud and Kubernetes costs?

**Memory Trick:** Right-Size → Autoscale → Reserved/Spot → Cleanup → Monitor

- **Right-sizing** – Most workloads are over-provisioned. Use VPA recommendations and historical data.
- **Autoscale aggressively** – Scale down at night/weekends, scale up on demand. Cluster autoscaler removes idle nodes.
- **Reserved instances + spot** – Reserved for baseline, spot for batch/non-critical. Mix for significant savings.
- **Cleanup discipline** – Old volumes, snapshots, load balancers, idle clusters drain budget silently.
- **Cost visibility per team/service** – Tags and showback dashboards make teams cost-conscious.

---

## Q7: How do you handle Kubernetes upgrades and patching in production?

**Memory Trick:** Plan → Test → Drain → Validate → Rollback Plan

- **Plan ahead** – K8s versions are supported for ~12 months. Don't fall behind; upgrades get harder with each skipped version.
- **Test in non-prod first** – Validate with real workloads in staging. API deprecations bite hardest.
- **Drain nodes carefully** – Respect pod disruption budgets, drain in batches, monitor health between batches.
- **Validate after each step** – Application health, metrics, traces. Don't proceed if anything is off.
- **Always have a rollback plan** – Especially for control plane upgrades. Practice it before you need it.

---

## Q8: How do you design for multi-region or high availability in the cloud?

**Memory Trick:** Multi-AZ → Multi-Region → Stateless → Data Strategy → DR

- **Multi-AZ first** – Spread workloads across availability zones in a region. Most outages are AZ-level.
- **Multi-region for higher SLA** – Active-active or active-passive based on RTO/RPO requirements.
- **Stateless services scale easily** – Stateless app tier behind load balancer, region-aware routing.
- **Data strategy is the hard part** – Replication, conflict resolution, consistency model. Plan for split-brain.
- **DR plan tested** – Run game days. A DR plan that's never executed is a fiction.

---

## Q9: How do you handle Docker image management and security?

**Memory Trick:** Minimal Base → Scan → Sign → Registry → Lifecycle

- **Minimal base images** – Distroless, Alpine, or scratch where possible. Smaller surface = smaller attack surface.
- **Image scanning** – Scan in CI (Trivy, Snyk, Prisma) and in registry. Block deploys on critical CVEs.
- **Sign images** – Cosign or Notary for image signing. Verify signatures before deploy.
- **Private registry** – Nexus, Artifactory, or cloud-native (ECR). No pulling random images from public registries.
- **Image lifecycle** – Retention policies for old images. Old, unscanned images are a liability.

---

## Q10: How would you migrate microservices from one platform to another (e.g., Cloud Foundry to Kubernetes)?

**Memory Trick:** Assess → Pilot → Migrate Iteratively → Decommission

- **Assess current state** – Inventory services, dependencies, configurations, traffic patterns.
- **Pilot with a low-risk service** – Migrate one or two non-critical services first. Build runbooks and tooling.
- **Migrate iteratively** – Service by service, with both platforms running in parallel. No big-bang cutovers.
- **Validate parity at each step** – Performance, error rates, integrations match the legacy platform before traffic shift.
- **Decommission deliberately** – Once all services and traffic are migrated, formally retire the old platform. Don't leave it running indefinitely.

> Strong closing line: *"Cloud and containers are tools, not goals. The goal is reliable, scalable, secure services — and the platform should serve that, not the other way around."*

---
