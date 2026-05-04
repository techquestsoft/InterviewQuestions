# Production Support, Observability & Reliability
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

---

## Q1: How do you guide your team in handling production incidents?

**Memory Trick:** Detect → Triage → Stabilize → Communicate → Learn

- **Detect early** – Alerts based on SLOs and customer-impact signals, not just CPU/memory thresholds.
- **Triage with clear ownership** – On-call engineer starts; incident commander assigned for sev-1/sev-2. Clear roles avoid chaos.
- **Stabilize before fixing forward** – Roll back, kill feature flag, reroute traffic. Restore service first, RCA second.
- **Communicate continuously** – Status page, war-room channel, regular updates to stakeholders (every 15-30 min for sev-1).
- **Learn from every incident** – Blameless RCA, action items with owners and dates, tracked to closure.

> Strong line: *"My goal is to move from incident handling to incident prevention."*

---

## Q2: How do you set up observability for production systems?

**Memory Trick:** Metrics → Logs → Traces → Dashboards → Alerts

- **Metrics** – RED metrics (Rate, Errors, Duration) per service; USE metrics (Utilization, Saturation, Errors) for resources.
- **Logs** – Structured (JSON), centralized (ELK/Splunk), with correlation IDs across services.
- **Traces** – Distributed tracing (OpenTelemetry, New Relic) to follow requests across services. Critical for microservices.
- **Dashboards** – Service-level dashboards for engineers, business-level dashboards for leadership.
- **Alerts** – On SLO violation and customer-impact signals, not on every spike. Alert fatigue is real and dangerous.

---

## Q3: How do you define and use SLIs, SLOs, and error budgets?

**Memory Trick:** SLI → SLO → Error Budget → Govern Releases

- **SLI (Service Level Indicator)** – What we measure: availability, latency, error rate. Should reflect customer experience, not just infra health.
- **SLO (Service Level Objective)** – Target for the SLI: e.g., 99.9% availability over 30 days, p95 latency < 300ms.
- **Error budget** – 100% minus SLO. Defines how much unreliability is acceptable.
- **Govern with error budget** – If budget burns too fast, prioritize reliability work over features until back on track.
- **Review regularly** – SLOs aren't set-and-forget. Review quarterly with product and business.

---

## Q4: Tell me about a production incident you led and what you learned.

**Memory Trick:** Situation → Action → Result → Learning

- **Situation** – Intermittent HTTP 500 spikes across services, root cause was an external authorization dependency timing out.
- **Initial response** – War room activated, traced via distributed tracing to identify the failing dependency.
- **Containment actions** – Added retries with exponential backoff, timeouts, and fallback logic. Improved circuit breaker thresholds.
- **Observability improvements** – Added per-dependency error tracking, alerting on external call failures, dashboards distinguishing internal vs external errors.
- **Learning** – Even self-recovering issues should be addressed proactively. Designing for failure of external dependencies isn't optional.

---

## Q5: How do you manage on-call rotations and reduce on-call burden?

**Memory Trick:** Fair Rotation → Runbooks → Reduce Toil → Pay Fairness

- **Fair rotation** – Predictable schedule, balanced across team, secondary on-call as backup.
- **Strong runbooks** – Every common alert has a runbook. Reduces cognitive load at 3 AM and speeds resolution.
- **Reduce toil at the source** – Track recurring alerts. If something pages 3+ times in a quarter, fix the underlying cause.
- **Auto-remediation where safe** – Self-healing for known patterns (auto-restart, auto-scale, etc.).
- **Recognize the burden** – On-call comp where applicable, time off after rough shifts, no judgment for waking the team for help.

---

## Q6: How do you do root cause analysis (RCA) effectively?

**Memory Trick:** Timeline → 5 Whys → Contributing Factors → Action Items → Track

- **Build a timeline** – What happened when. Logs, alerts, deploys, manual actions all in one timeline.
- **5 Whys** – Don't stop at "the service crashed." Why did it crash? Why wasn't it caught? Why didn't the alert fire?
- **Identify contributing factors** – Often many causes: code bug + bad alert + slow rollback + unclear runbook. Address all.
- **Action items with owners and dates** – Vague items don't get done. Specific, owned, time-bound.
- **Track to closure** – Review action items in retros and team reviews. Open RCA actions are a leading indicator of future incidents.

---

## Q7: How do you balance reliability work with feature delivery?

**Memory Trick:** Allocate → Error Budget → Toil Tracking → Make It Visible

- **Dedicated capacity** – ~15-20% of every sprint for reliability, observability, tech debt. Non-negotiable.
- **Error budget governance** – When SLO is at risk, reliability work takes priority. Not negotiable with product.
- **Track operational toil** – Time spent on incidents, on-call escalations, manual ops. Toil > 30% means we have a reliability problem.
- **Make reliability visible to leadership** – Dashboards showing SLO trends, incident counts, MTTR. Leaders fund what they see.
- **Tie reliability to business outcomes** – Outage cost, customer churn, regulatory exposure. Reliability is a business case.

---

## Q8: How do you handle preventive maintenance — patching, upgrades, security fixes?

**Memory Trick:** Track → Prioritize → Plan → Execute → Validate

- **Track all platforms and dependencies** – Inventory of versions, EOL dates, known CVEs across services.
- **Prioritize by risk** – Critical CVEs, supported versions, regulatory requirements first.
- **Plan into the roadmap** – Patching is not free. Goes into sprint planning like any other work.
- **Execute with controlled rollout** – Same care as feature deploys: canary, monitor, rollback ready.
- **Validate after** – Smoke tests, monitoring for regression. Don't assume "it's just a patch."

---

## Q9: How do you design systems for resilience against failures?

**Memory Trick:** Timeout → Retry → Circuit Breaker → Fallback → Bulkhead

- **Timeouts everywhere** – No call should hang indefinitely. Set timeouts at the service, client, and DB level.
- **Retries with backoff and jitter** – For transient failures. Exponential backoff prevents retry storms.
- **Circuit breakers** – Fail fast when a dependency is down rather than waiting for timeouts. Resilience4j or similar.
- **Fallback strategies** – Cached data, default response, degraded functionality. Better than total failure.
- **Bulkheads** – Isolate failures by partitioning resources (thread pools, connection pools per dependency). One bad dependency doesn't take down the whole service.

---

## Q10: How do you drive a culture of operational excellence in your team?

**Memory Trick:** Ownership → Metrics → Blameless → Invest

- **You build it, you run it** – Engineers own their services in production, not just in dev. Builds empathy for ops.
- **Metrics-driven** – Service health, SLO compliance, incident count, MTTR are reviewed in team reviews.
- **Blameless culture** – Mistakes are learning opportunities, not blame opportunities. Engineers who fear blame hide problems.
- **Invest in operational tooling** – Better dashboards, runbooks, automation. Pay it forward to your future on-call self.
- **Celebrate ops wins** – Successful incident response, reduced MTTR, improved SLO compliance — recognize them like feature wins.

> Strong closing line: *"Reliability is a product feature. The best engineering teams I've led treated production health with the same rigor as customer features."*

---
