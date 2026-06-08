# FILE 3 OF 8 — DELIVERY, EXECUTION & KPIs

> **Rule 1:** Lead with outcomes, not process. Numbers before frameworks.
> **Rule 2:** Executive KPI questions need 3 clean metrics — no jargon. Two sets memorized (Q16).
> **Rule 3:** Spillovers, sprint discipline, tech debt — every answer needs a real example.

---

## CROSS-FILE INDEX

This file owns: delivery management, SAFe, sprint discipline, tech debt, KPIs, productivity measurement.
- People-side accountability → File 02
- Production reliability KPIs (MTTR/MTTD) → File 06
- AI productivity ROI → File 07

---

# SECTION A — DELIVERY & EXECUTION

## Q1: How do you manage delivery end-to-end?

**Memory Hook:** Planning → Execution Discipline → Risk Management → Delivery Predictability

> Three integrated practices.
>
> **Planning.** I align the roadmap with product every quarter, balancing feature work, tech debt, and compliance. I do not let product own the entire backlog — deferred tech debt compounds into incidents. I write the major features and user stories myself.
>
> **Execution discipline.** Stories small enough to complete in one sprint. If a story requires two sprints, it gets broken into two stories. I watch burndowns mid-sprint. If on day 5 we have closed only 20% of stories, I convene a triage immediately — not at retrospective.
>
> **Risk management.** I identify dependencies — internal and external — early and track them weekly. Cross-team blockers I own and escalate myself. I do not leave engineers navigating the org alone.
>
> The outcome is delivery predictability — not every sprint is perfect, but **stakeholders never get surprised by delays. Surprises are the failure, not slippage itself.**

---

## Q2: What is RAID tracking and how do you use it?

**Memory Hook:** Risks → Assumptions → Issues → Dependencies

> RAID tracking — Risks, Assumptions, Issues, Dependencies — is my primary governance mechanism for large engineering programs.
>
> **Risks** — tracked with probability, impact, owner, mitigation.
> **Assumptions** — what we are treating as true; flagged for validation. Hidden assumptions are a common cause of slippage.
> **Issues** — things that have already gone wrong; tracked with owner and resolution date.
> **Dependencies** — what we need from other teams or vendors; tracked weekly.
>
> Most enterprise delivery slippages happen at integration points between teams. RAID makes those visible before they become blockers.

---

## Q3: How do you ensure delivery predictability?

**Memory Hook:** Right-Sized Work → Track Planned vs Actual Weekly → 20% Sprint Capacity for Debt

> Three things drive predictability.
>
> **Right-sized work** — stories too large are the primary cause of spillovers. Nothing spans more than one sprint. If it does, it gets broken down.
>
> **Track planned vs actual weekly** — I do not wait for retrospective to find out we missed. Burndown flat on day 6 triggers an immediate question: dependency, unclear requirement, or underestimate? Resolved the same day.
>
> **Balance feature and tech work** — teams carrying heavy debt deliver unpredictably because every feature takes longer than estimated. I reserve **20% of sprint capacity for debt**. That 20% pays back as fewer mid-sprint surprises.
>
> Teams that protect the 20% consistently hit >85% delivery predictability. Teams that defer all debt to "next quarter" rarely do.

---

## Q4: How do you prioritize work across competing demands?

**Memory Hook:** Production Stability → Committed Features → Tech Debt → Business Impact Over Volume

> My hierarchy: production stability and customer impact first, then committed feature work, then tech debt and platform investments.
>
> Within features: business impact as the ranking criterion — not loudness of the requestor. I translate feature requests into business value (revenue impact, customer satisfaction, compliance risk) and rank accordingly.
>
> Trade-offs presented explicitly: if a compliance item requires pushing a feature one sprint, I show the cost of not doing the compliance item alongside the cost of the delay. The business makes an informed call.
>
> What I avoid: letting urgency substitute for importance. **The loudest request is not always the most valuable one.**

---

## Q5: How do you handle sprint spillovers?

**Memory Hook:** Three Causes → Burndown Mid-Sprint → Triage Immediately

> Three causes, three responses.
>
> **Underestimated complexity** — most common in legacy code. Immediate scope triage: keep the main flow this sprint, move alternate flows to next sprint explicitly. Documented in Jira, not hidden.
>
> **External dependency blocked** — move the story back to backlog immediately. Blocked work carried as in-progress distorts the burndown and the picture.
>
> **Wrong estimate** — retrospective item. Why did we misjudge, what do we calibrate differently. No blame — just calibration.
>
> At Cerner, spillovers happened due to legacy code complexity. We addressed it through **deeper grooming with senior engineers walking newer ones through legacy patterns before sprint planning**. Spillovers dropped significantly within two sprints.

---

## Q6: How do you manage releases?

**Memory Hook:** Approval → Validation → Deployment Strategy → Post-Deploy Monitoring

> Four stages, plus the Oracle Cerner-specific chain as a concrete example.
>
> **Approval** — CAB (Change Approval Board). Submit a Remedy change request with test evidence, rollback plan, and blast-radius assessment. Without CAB approval, nothing goes to production.
>
> **Validation** — all automated gates pass: code coverage, security scan, integration tests. Release readiness is binary, not "mostly ready."
>
> **Deployment strategy** — I pick the strategy based on risk:
> - **Blue-Green** for major releases — two live environments, cut traffic over after validation, instant rollback
> - **Canary** for new features or model rollouts — deploy to a subset of clients, validate, expand
> - **Rolling** for routine low-risk releases
>
> V3 ML model rollout at Cerner used canary — deployed to 10% of clients, validated prediction accuracy matched baseline, then expanded to 100%. Zero accuracy regression.
>
> **Post-deploy monitoring** — I watch dashboards for 24 hours post-release: error rates, latency, throughput. If something spikes, we roll back rather than investigating in production.
>
> **Oracle Cerner formal chain (OHRM → HDI CAB → Remedy CR → JFORMs → TTP):**
>
> Five formal gates between code-complete and production:
> - **OHRM** — architecture/platform changes need sign-off before implementation begins
> - **HDI CAB** — weekly review: blast radius, rollback plan, test evidence
> - **Remedy CR** — formal ticket, audit trail
> - **JFORMs** — QA testing sign-off, regression complete
> - **TTP (Transfer to Production)** — I sign off, then Spinnaker deploys
>
> Beyond release-time: monthly Dev/Ops Quality Reviews, Ops Maturity Assessments, yearly audits. In clinical decision support, the cost of a production incident is regulatory and clinical — not just operational.

---

## Q7: How do you balance execution and quality?

**Memory Hook:** In the Pipeline → In Code Review → In Sprint Review → Feedback Loop

> Quality is not a phase at the end of delivery — it is built into every step.
>
> **In the pipeline:** 90% code coverage enforced, SonarQube quality gates, Fortify security scanning. Not suggestions — blockers. Code that fails does not progress.
>
> **In code review:** I am the mandatory second-level approver. I check architectural conformance, boundary violations, exception handling — not formatting.
>
> **In sprint review:** if the demo does not reflect what was committed, that is a quality signal even if all automated gates passed.
>
> **Feedback loop:** every incident produces at least one structural fix, not just a workaround. Production incidents go back into the backlog as corrective actions.

---

## Q8: What do you do if quality drops while scaling?

**Memory Hook:** Clarify Scaling Type → Team / System / Delivery → Individual vs Systemic → Fix the Gap

> Three flavors of scaling, each with a different quality-drop pattern.
>
> **Team scaling (more engineers)** — quality drops because standards do not propagate automatically. Fix: structured onboarding with paired reviews for the first month, coding standards enforced in CI gates not tribal knowledge, senior engineer ratio protected.
>
> **System scaling (more load, more customers)** — quality drops because edge cases that did not matter at small scale now surface. Fix: load testing as a gate before major customer onboarding, observability on p99 and tail latency not averages.
>
> **Delivery scaling (shipping faster)** — quality drops because pressure compresses review and testing time. Fix: tighten release gates — a release introducing known P1/P2 defects does not go out — enforce coverage thresholds, protect the 20% tech-debt allocation.
>
> Common thread: first question is always individual or systemic? If multiple engineers or services show the same defect pattern, it is a system problem — fix the system before coaching individuals.

---

## Q9: How do you manage technical debt?

**Memory Hook:** The 20% Rule → Visible Register → WSJF Prioritization → Platform-Health Sprints

> Tech debt is a first-class backlog item, not a conversation that happens when there is spare capacity — which there never is.
>
> **The 20% rule.** I reserve 20% of every sprint for debt, refactoring, and non-functional improvements. When stakeholders push back: debt not addressed today costs three times more in six months.
>
> **Visible register.** Each item has: estimated cost of delay, risk rating, owner, effort estimate. Visible to stakeholders. When someone asks why a feature is slower than expected, I show them the debt we are carrying.
>
> **WSJF prioritization.** Weighted Shortest Job First — high-risk, low-effort items first.
>
> **Platform-health sprints.** For significant debt needing sustained focus: one dedicated sprint per quarter. No feature work — debt, performance, observability only.
>
> At Cerner, a legacy Oracle integration caused 40% of production incidents from one layer. One platform-health sprint per quarter over two quarters **dropped the incident rate from that layer by 60%.**

---

## Q10: How do you balance modernization with business delivery commitments?

**Memory Hook:** Phased Modernization → Sequenced Value → Alongside Business Commitments

> I do not treat modernization and business delivery as competing. Phased modernization — where each phase delivers value independently and reduces risk for the next — runs alongside business commitments, not in dedicated cutover windows.
>
> AWS-to-OCI migration at Cerner: sequenced service-by-service, ran both environments in parallel during cutover, avoided any large disruptive rewrite. **Zero customer-impacting outages during migration. Active client commitments continued uninterrupted.**

---

## Q11: How do you handle multiple competing initiatives / large cross-functional programs?

**Memory Hook:** Workstream-Based Management → Named Owners → Weekly Integration Sync → Governance

> **Workstream-based management.** Break the portfolio into clearly bounded workstreams. Each workstream gets a named owner — not me. I become the program-level integrator, not the bottleneck.
>
> Four pillars for large programs:
>
> **Structure** — decompose into workstreams with clear scope, dependencies, and integration points.
>
> **Ownership** — each workstream has a named lead accountable for delivery. I am program-level, not workstream-level.
>
> **Governance** — weekly 30-minute integration sync where workstream leads surface cross-team conflicts. Monthly architecture reviews to keep workstreams aligned.
>
> **Visibility** — single program dashboard accessible to stakeholders. Status, risks, decisions needed — all in one place.
>
> At Cerner, I ran three simultaneous workstreams: V3 ML transformation, AWS-to-OCI migration, CrowdStrike security rollout. Three named owners. One weekly integration sync. **That single ceremony surfaced ~80% of cross-workstream conflicts before they became blockers.**

---

## Q12: How do you align delivery with business outcomes?

**Memory Hook:** Map to Business Outcome → Track Outcome Metrics → Communicate Back to Engineering

> Map every engineering investment to a business outcome at the start. Not "build feature X" but "reduce care manager time per patient by 15% so they can serve 20% more patients."
>
> Track outcome metrics, not just delivery metrics. Did we ship the feature on time? Yes — but did adoption hit the predicted level? Did the business outcome materialize within 1 to 2 quarters of release?
>
> Communicate outcomes back to engineering. Engineers who see the business impact of their work stay engaged. Engineers who only see story-point throughput burn out faster.
>
> Care Coordination products at Cerner generated **$20M+ annual revenue** serving 120+ customers — every feature was tied to a measurable clinical or operational outcome.

---

# SECTION B — AGILE & DELIVERY FRAMEWORKS

## Q13: How do you scale delivery using SAFe?

**Memory Hook:** PI Planning Quality → Dual Tracking → Dependency Management → Scope Protection

> SAFe works when multiple teams need to coordinate toward a shared product increment. Four specific failure modes I prevent:
>
> **PI Planning quality.** Teams must enter with refined, estimated backlog items — not vague stories. I enforce a refinement checkpoint two weeks before PI planning.
>
> **Dual tracking.** Team level (sprint velocity, commitment accuracy) and program level (PI objective completion rate). If program completion drops below 80%, I investigate — planning failure or execution failure have different causes and different fixes.
>
> **Dependency management.** Weekly scrum-of-scrums where team leads surface cross-team blockers. I own resolving cross-team blockers myself rather than leaving engineers to navigate the org hierarchy alone.
>
> **Scope protection.** Business stakeholders must go through a formal change process to swap scope after PI planning. Ad-hoc additions mid-PI destroy objectives and team morale.

---

## Q14: How do you run effective retrospectives?

**Memory Hook:** Preserve → Change → Specific Owners and Dates

> The purpose of retrospective is structural improvement, not catharsis.
>
> Three parts: what produced good outcomes (preserve), what caused friction (change), what we will do differently next sprint with **specific owners and dates**.
>
> The third part is what most teams skip — vague action items with no owners. I enforce specifics: "Add integration test for payment flow — owner: [name] — done by next sprint review."
>
> I track retrospective action items the same way I track sprint commitments. If we are not completing retrospective actions, we are running them as therapy sessions, not improvement cycles.

---

# SECTION C — KPIs & METRICS

## Q15: How do you define and use KPIs for your team?

**Memory Hook:** Business Impact → Engineering Efficiency → System Reliability and Quality (Balanced)

> I define KPIs across three dimensions so engineering aligns with business outcomes rather than measuring activity.
>
> **Business impact** — reducing cloud infrastructure cost by 15%, improving feature adoption by 20%, or improving customer onboarding completion. Tells the business whether engineering is creating value.
>
> **Engineering efficiency** — lead time for changes, deployment frequency, delivery predictability. Example target: reducing lead time from two weeks to one week through better CI/CD automation.
>
> **System reliability and quality** — SLO adherence, incident rate, MTTR, defect escape rate. Example target: defect escape rate below 2% for features my team owns this quarter.
>
> **Balanced scorecard, not a single metric.** Velocity alone can be gamed. Deployment frequency alone ignores quality. The combination is harder to game and more informative.
>
> **Strong close:** "I use KPIs to drive outcomes, not just measure activity."

---

## Q16: Give me 3 executive-level KPIs

**Memory Hook:** TWO SETS — choose based on the question framing

> **SET A — Operational / Reliability framing**
> **Memory Hook:** MTTD + MTTR + CFR
>
> One — **Mean Time to Detect (MTTD).** How long between a problem occurring and alerting catching it. Target: under 5 minutes for P1. Long MTTD means monitoring is reactive.
>
> Two — **Mean Time to Restore (MTTR).** How long from detection to full service restoration. Target: under 30 minutes for P1. Fast restoration means runbooks, rollback capabilities, and on-call processes are mature.
>
> Three — **Change Failure Rate (CFR).** What percentage of production deployments cause an incident or require rollback within 24 hours. Target: below 5%. Direct measure of deployment quality and testing maturity.
>
> These three together tell the executive: how quickly we find problems, how quickly we fix them, and how well we prevent them.

> **SET B — Quality / Delivery framing**
> **Memory Hook:** Escaped Defect Rate + MTTR + Delivery Predictability
>
> One — **Escaped Defect Rate.** Of all defects found, what percentage reached customers vs caught internally. Target: below 5%. Tells leadership whether quality gates are working.
>
> Two — **MTTR.** When something breaks, how long until customers are back. Target: under 30 minutes for P1.
>
> Three — **Delivery Predictability.** Of features committed for the quarter, what percentage shipped on the committed date. Target: above 85%.

> | If the question is about... | Use |
> |---|---|
> | Operational health, incidents, reliability | **Set A** (MTTD + MTTR + CFR) |
> | Quality maturity, delivery, engineering health | **Set B** (Escaped Defect + MTTR + Predictability) |
> | Generic "executive KPIs" with no framing | **Set A** — DORA-aligned, more universally recognized |

---

## Q17: How do you measure productivity and performance?

**Memory Hook:** Outcomes Over Activity → Individual Signals → Trends Over Points

> I focus on outcomes, not activity metrics. Sprint velocity tells me something changed — it is a signal, not a verdict. A team completing 50 points of low-value work is less productive than one completing 30 points of high-impact work.
>
> For individual performance: delivery reliability, output quality (defect rate, rework rate), and ownership behavior (flagging risks early, unblocking peers).
>
> Track trends over at least two sprint cycles before drawing conclusions. One missed sprint is noise. A pattern is signal.

---

## Q18: How do you measure ROI of engineering initiatives?

**Memory Hook:** Cost → Speed → Business Value

> Three levels.
>
> **Cost** — establish a baseline before the initiative and measure delta after. The OpenShift-to-Kubernetes migration saved **$5M annually** — directly measurable.
>
> **Speed** — if implementing a feature of known complexity previously cost X hours and now costs 0.8X, that is 20% ROI — equivalent to additional capacity within the same budget.
>
> **Business value** — track the stated business outcome. Did the feature reduce care-manager time? Increase risk score accuracy? Engineering ROI is only complete when business outcome is measured, not just delivery.
>
> **Strong close:** "I translate engineering improvements into measurable business value."

---

## Q19: How do you prevent KPI gaming?

**Memory Hook:** Balanced Scorecard → Cross-Validate → Trends Over Quarters → Outcomes Over Activity

> Single metrics get gamed. Balanced scorecards are much harder to game.
>
> Velocity alone → engineers inflate story points. Add quality metrics alongside, and inflating velocity at quality cost becomes visible.
>
> Cross-validate: velocity up + defect rate up = velocity number is suspect. Deployment frequency up + MTTR up = releasing too fast without testing.
>
> Focus on trends over at least two quarters — point-in-time numbers are easy to manipulate.
>
> **Prioritize outcomes over activity. Features shipped is activity. Customer adoption of those features is outcome.**

---

## Q20: How do you align KPIs with business goals?

**Memory Hook:** Align → Map to Outcomes → Review Quarterly

> Align KPIs with product and business priorities — not engineering preference. Map engineering metrics to business outcomes — every metric should answer "so what does this mean for the business?" Review KPIs with stakeholders quarterly — adjust based on changing priorities.
>
> When the business priority shifts from "ship faster" to "improve stability," the KPI portfolio must shift too — otherwise engineering is optimizing for last quarter's goals.

---

## Q21: How do you manage a platform where the deadline is regulatory and cannot slip?

**Memory Hook:** Fixed Deadline → Alert at 70% Window → Escalation Protocol → Crisis Parallel Tracks

> Regulatory deadlines are different from sprint deadlines. You cannot negotiate a two-day extension with the regulator. Three modes: steady-state, early-warning, and crisis.
>
> **Steady-State.** Pipeline SLAs expressed as countdown to regulatory cutoff — not just job duration. Track all upstream pipelines against their must-complete-by times. Daily reconciliation reports showing source vs processed counts. Any variance investigated before the next pipeline run — never carry forward unresolved variance.
>
> **Early Warning.** Alert at **70% of the SLA window remaining** — not at breach. If a report is due at 8 AM and the pipeline has not completed by 5 AM, escalate immediately. Defined escalation tree: pipeline engineer → data platform lead → EM → business stakeholder. Each level has a response time SLA.
>
> **Crisis.** Incident commander model — one person owns resolution. Parallel tracks: technical team diagnoses and resolves; I communicate to stakeholders on 15-minute cadence. If a report cannot be filed accurately, the **business** makes the regulatory call — not engineering. Engineering provides accurate status and options.
>
> **Cross-reference:** data-layer specifics (Iceberg snapshot locking, reconciliation sign-off gate) → File 08 Section G.

---

# QUICK REFERENCE — MEMORY HOOKS

| Q | Topic | Hook |
|---|---|---|
| Q1 | Manage delivery end-to-end | Planning → Execution Discipline → Risk Management → Delivery Predictability |
| Q2 | RAID tracking | Risks → Assumptions → Issues → Dependencies |
| Q3 | Delivery predictability | Right-Sized Work → Track Weekly → 20% Sprint Capacity for Debt |
| Q4 | Prioritize competing demands | Production Stability → Committed Features → Tech Debt → Business Impact Over Volume |
| Q5 | Sprint spillovers | Three Causes → Burndown Mid-Sprint → Triage Immediately |
| Q6 | Manage releases | Approval → Validation → Deployment Strategy → Post-Deploy Monitoring |
| Q7 | Execution vs quality | In the Pipeline → In Code Review → In Sprint Review → Feedback Loop |
| Q8 | Quality drops while scaling | Clarify Type → Team/System/Delivery → Individual vs Systemic → Fix the Gap |
| Q9 | Technical debt | 20% Rule → Visible Register → WSJF → Platform-Health Sprints |
| Q10 | Modernization vs delivery | Phased Modernization → Sequenced Value → Alongside Business Commitments |
| Q11 | Multiple initiatives / large programs | Workstream-Based → Named Owners → Weekly Integration Sync → Governance |
| Q12 | Delivery to business outcomes | Map to Outcome → Track Outcome Metrics → Communicate Back |
| Q13 | SAFe | PI Planning Quality → Dual Tracking → Dependency Management → Scope Protection |
| Q14 | Retrospectives | Preserve → Change → Specific Owners and Dates |
| Q15 | Team KPIs | Business Impact → Engineering Efficiency → System Reliability (Balanced) |
| Q16 | 3 executive KPIs | **Set A**: MTTD + MTTR + CFR / **Set B**: Escaped Defect + MTTR + Predictability |
| Q17 | Productivity & performance | Outcomes Over Activity → Individual Signals → Trends Over Points |
| Q18 | ROI of initiatives | Cost → Speed → Business Value |
| Q19 | Prevent KPI gaming | Balanced Scorecard → Cross-Validate → Trends Over Quarters → Outcomes |
| Q20 | Align KPIs to business | Align → Map to Outcomes → Review Quarterly |
| Q21 | Regulatory deadline management | Fixed Deadline → Alert at 70% → Escalation Protocol → Crisis Parallel Tracks |
| Q22 | Upstream cannot deliver data — what do you do? | Confirm Upstream → Copy-Forward Fallback → Communicate What’s Missing → Structural Fix Next Sprint |

---

## Q22: Upstream system confirms it cannot deliver data today. SLA is on you. What do you do?

**Memory Hook:** Confirm Upstream → Copy-Forward Fallback → Communicate What’s Missing → Structural Fix Next Sprint

> This is distinct from Q21 (pipeline failure you can fix). This is a confirmed external dependency failure that is not recoverable today. The upstream will not send the data. The SLA with your stakeholders still exists.

> **Core Answer**
>
> Once upstream confirms they cannot deliver — the data is not coming today — I stop trying to solve the upstream problem and focus entirely on what is within my control.
>
> **Step 1: Copy-forward fallback.** Use the last known good dataset — yesterday's or the most recent validated snapshot. Identify exactly which fields or attributes will be stale or missing. Document that explicitly.
>
> **Step 2: Communicate precisely, not generically.** Do not say the report is unavailable. Say: "Today's report is available. Two attributes — [specific fields] — reflect yesterday's data because the upstream feed was not received. All other data is current as of today's run. The upstream team is accountable for this gap and the SLA timeline is being reset." Stakeholders can usually work with stale data on two attributes. They cannot work with a missing report.
>
> **Step 3: Hold the right party accountable.** The upstream team owns the miss. I make sure that is clearly documented — in the incident record, in the communication to leadership — so the SLA impact is attributed correctly. Engineering does not absorb accountability for a dependency failure.
>
> **Step 4: Structural fix.** After the incident, I review whether a copy-forward mechanism is formally designed and documented, or whether it was improvised. If improvised, it becomes a backlog item — design and test the fallback before the next failure, not during it.

> **Rule:** The copy-forward answer is always better than "we cannot release today." Partial, accurate data with clear communication about what is missing is almost always more useful to the business than a blocked report.

---

*File 3 of 8 — Delivery, Execution & KPIs*
*Updated June 2026 — added Q22 (upstream cannot deliver data — copy-forward fallback) from Wells Fargo Round 1*
