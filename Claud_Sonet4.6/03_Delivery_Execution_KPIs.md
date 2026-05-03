# Interview Prep — File 3 of 7
# Delivery, Execution & KPIs

> **Rule 1:** Lead with outcomes, not process. Numbers before frameworks.
> **Rule 2:** Executive KPI questions need 3 clean metrics — no jargon. Two sets memorized below.
> **Rule 3:** Spillovers, sprint discipline, tech debt — every answer needs a real example.

---

## CROSS-FILE INDEX

This file owns: delivery management, SAFe, sprint discipline, tech debt, KPIs, productivity measurement.
- People-side accountability → File 02
- Production reliability KPIs (MTTR/MTTD context) → File 05
- AI productivity ROI → File 06

---

## SECTION A — DELIVERY & EXECUTION

---

### Q1: How do you manage delivery end-to-end?

**Memory Hook:** Plan → Break → Execute → Risk → Deliver

> "Three integrated practices.
>
> **Planning.** I align the roadmap with product priorities every quarter, balancing feature work, tech debt, and compliance items. I do not let product own the entire backlog — deferred tech debt compounds into incidents.
>
> **Execution discipline.** I break work into user stories small enough to complete in one sprint. I use burndown charts mid-sprint, not as a lagging indicator but as an early warning. If on day 5 we have closed only 20% of stories, I convene a triage immediately.
>
> **Risk management.** I identify dependencies early — internal and external — and track them weekly. Cross-team blockers I own and escalate myself. I do not leave engineers navigating the org alone.
>
> The outcome is delivery predictability — not every sprint is perfect, but stakeholders never get surprised by delays."

---

### Q2: How do you ensure delivery predictability?

**Memory Hook:** Small Work → Track → Balance

> "Three things drive predictability.
>
> Right-sized work — stories too large are the primary cause of spillovers. I enforce decomposition so nothing spans more than one sprint. If a story requires two sprints, it gets broken into two stories.
>
> Track planned vs actual weekly — I do not wait for retrospective to find out we missed. If burndown is flat on day 6, I ask what is blocking — dependency, unclear requirement, underestimate — and resolve it.
>
> Balance feature and tech work — teams carrying heavy debt deliver unpredictably because every feature takes longer than estimated. I reserve 20% of sprint capacity for debt. That 20% pays back as fewer mid-sprint surprises."

---

### Q3: How do you prioritize work across competing demands?

**Memory Hook:** Production → Features → Platform

> "My hierarchy: production stability and customer impact first, then committed feature work, then tech debt and platform investments.
>
> Within features, I use business impact as the ranking criterion — not loudness of the requestor. I translate feature requests into business value (revenue impact, customer satisfaction, compliance risk) and rank accordingly.
>
> Trade-offs presented explicitly: if a compliance item requires pushing a feature one sprint, I show the cost of not doing the compliance item — regulatory risk, audit exposure — alongside the cost of the delay. The business makes an informed call.
>
> What I avoid: letting urgency substitute for importance. The loudest request is not always the most valuable one."

---

### Q4: How do you handle sprint spillovers?

**Memory Hook:** 3 Causes, 3 Responses, Burndown Mid-Sprint

> "Three causes, three responses.
>
> **Underestimated complexity** — most common in legacy code. When a developer discovers it mid-sprint, we do an immediate scope triage: keep the main flow this sprint, move alternate flows to next sprint explicitly. Documented in Jira, not hidden.
>
> **External dependency blocked** — move the story back to backlog immediately. Do not carry blocked work as in-progress; it distorts the burndown.
>
> **Wrong estimate** — retrospective item. Discuss why we misjudged, update calibration for similar complexity. No blame — just calibration.
>
> I use burndown mid-sprint, not just at end-of-sprint. If on day 5 of 10 we have closed only 20% of stories, I triage immediately. I do not wait for retro.
>
> Real example at Cerner: spillovers happened due to legacy code complexity — V1/V2 had been built over multiple years with patterns that surprised newer engineers. We addressed it through deeper grooming with senior engineers walking newer ones through the legacy patterns before sprint planning."

---

### Q5: How do you manage releases?

**Memory Hook:** Approve → Validate → Deploy → Monitor

> "Four stages.
>
> **Approval** — CAB (Change Approval Board). We submit a Remedy change request with test evidence, rollback plan, and blast-radius assessment. Without CAB approval, nothing goes to production.
>
> **Validation** — all automated gates pass: code coverage, security scan, integration tests. Manual QA for edge cases that cannot be automated. Release readiness is binary, not 'mostly ready.'
>
> **Deployment** — Spinnaker multi-stage pipeline. For major releases, blue-green deployment — two live environments, cut traffic over after validation. Rollback is instant. For incremental changes, canary — deploy to subset of clients, monitor, expand.
>
> **Monitoring** — I do not consider a release done at deployment. I watch dashboards for 24 hours post-release: error rates, latency, throughput. If something spikes, we roll back rather than investigating in production.
>
> Real example: V3 ML model rollout at Cerner used canary — deployed to 10% of clients, validated prediction accuracy matched baseline, then expanded to 100%. Zero accuracy regression."

---

### Q5a: Walk me through the actual release management chain at Oracle Cerner

If interviewer probes the formal governance:

**Memory Hook:** OHRM → HDI CAB → Remedy CR → JFORMs → TTP

> "Five formal gates between code-complete and production.
>
> **OHRM (Oracle Health Release Management)** — for any architecture or platform-level change, OHRM approval is required before implementation begins. I work with enterprise architecture, present the proposal, get sign-off.
>
> **HDI CAB (Change Approval Board)** — weekly review for upcoming releases across the Health Data Intelligence platform. I submit the change request, present blast radius, rollback plan, and test evidence.
>
> **Remedy CR (Change Request)** — formal ticket in Remedy with all artifacts attached. CAB approval is recorded here for audit trail.
>
> **JFORMs approval** — testing sign-off documentation. QA confirms test coverage, edge cases validated, regression complete.
>
> **TTP (Transfer to Production)** — final manual authorization. Engineering manager signs off, then deployment runs through Spinnaker.
>
> Beyond release-time governance, ongoing quality is enforced through monthly Dev and Ops Quality Reviews, yearly internal and external audits, Solution Record Reviews, and Ops Maturity Assessments. These are the platform-level continuous-quality mechanisms.
>
> The discipline this requires can feel heavy, but it's the right level of governance for healthcare. The cost of a production incident in clinical decision support is regulatory, not just operational."

---

### Q6: How do you balance execution and quality?

**Memory Hook:** Execute + Control + Feedback Loop

> "Quality is not a phase at the end of delivery — it is built into every step.
>
> In the pipeline: 90% code coverage enforced, SonarQube quality gates, Fortify security scanning. Not suggestions — blockers. Code that fails does not progress.
>
> In code review: I am the mandatory second-level approver. I check architectural conformance, boundary violations, exception handling. Not formatting — automated tools handle that.
>
> In sprint review: I check demo quality. If the demo does not reflect what was committed, that is a quality signal even if code passes all automated gates.
>
> Feedback loop: production incidents go back into the backlog as corrective actions. Every incident produces at least one structural fix, not just a workaround."

---

### Q7: What do you do if quality drops while scaling?

**Memory Hook:** Diagnose Systemic vs Individual → Tighten Gates → Invest in Maturity

> "First question: is this individual or systemic? If multiple engineers are introducing defects, it is a system problem — unclear requirements, insufficient test coverage standards, or time pressure that compressed review.
>
> Analyze by defect type: test coverage gaps? Integration issues? Regressions from tech debt? Different causes need different fixes.
>
> For immediate control: tighten release gates. A release introducing known P1/P2 defects does not go out — period. Non-negotiable even under business pressure.
>
> For improvement: enforce the practices that were slipping — coverage thresholds, integration tests, design reviews before implementation.
>
> For prevention: invest in engineering maturity. Defect rates are lagging indicators of process and skill gaps. Fix the gap, not just the symptom."

---

### Q8: How do you manage technical debt?

**Memory Hook:** 20% Rule → Visible Register → WSJF → Platform Health Sprints

> "Tech debt is a first-class backlog item, not a conversation that happens when there is spare capacity (which there never is).
>
> **The 20% rule.** I reserve 20% of every sprint for debt, refactoring, and non-functional improvements. Non-negotiable. When stakeholders push back, I use the compound-interest analogy: debt not addressed today costs three times more in six months.
>
> **Visible register.** Tech debt register with four fields per item: estimated cost of delay, risk rating, owner, effort estimate. Visible to stakeholders. When someone asks why a feature is slower than expected, I show them the debt we are carrying.
>
> **WSJF prioritization.** Weighted Shortest Job First — high-risk, low-effort items first. The most dangerous debt gets resolved without requiring long dedicated sprints.
>
> **Platform-health sprints.** For significant debt requiring sustained focus, I negotiate one dedicated sprint per quarter. No feature work — debt, performance, observability only.
>
> Real example: at Cerner, a legacy Oracle integration was causing 40% of production incidents from one layer. One platform-health sprint per quarter, over two quarters, dropped the incident rate from that layer by 60%."

---

### Q9: How do you handle multiple competing initiatives?

**Memory Hook:** Split → Own → Track

> "Workstream-based management.
>
> Break the portfolio into clearly bounded workstreams. Each workstream gets a named owner — not me. I become the program-level integrator, not the bottleneck.
>
> Track each workstream against its objectives weekly. The integration points between workstreams get explicit attention — that is where things break.
>
> Stakeholder communication happens at program level, not per-workstream. One coherent narrative to leadership, not three competing ones.
>
> Real example at Cerner: I was simultaneously running V3 ML transformation, AWS-to-OCI cloud migration, and CrowdStrike security rollout. Three workstreams, three named owners. I ran a weekly 30-minute integration sync — that single ceremony surfaced 80% of the cross-workstream conflicts before they became blockers."

---

### Q10: How do you align delivery with business outcomes?

**Memory Hook:** Map → Track → Communicate

> "Map every engineering investment to a business outcome at the start. Not 'build feature X' — 'reduce care manager time per patient by 15% so they can serve 20% more patients.' Engineering work without a stated business outcome is hard to justify and easy to deprioritize.
>
> Track outcome metrics, not just delivery metrics. Did we ship the feature on time? Yes — but did adoption hit the predicted level? Did the business outcome materialize within 1 to 2 quarters of release?
>
> Communicate outcomes back to engineering. Engineers who see the business impact of their work stay engaged. Engineers who only see story-point throughput burn out faster."

---

## SECTION B — AGILE & DELIVERY FRAMEWORKS

---

### Q11: How do you scale delivery using SAFe?

**Memory Hook:** Pre-work → Dual Tracking → Own Dependencies → Protect Scope

> "SAFe works well when multiple teams need to coordinate toward a shared product increment. The failure modes are well-known — I prevent them specifically.
>
> **PI Planning quality.** The ceremony is only as good as the pre-work. Teams must enter with refined, estimated backlog items — not vague stories. I enforce a refinement checkpoint two weeks before PI planning.
>
> **Dual tracking.** I track at team level (sprint velocity, commitment accuracy) and program level (PI objective completion rate). If program completion drops below 80%, I investigate whether it is a planning failure or execution failure — different causes, different fixes.
>
> **Dependency management** — the most common SAFe failure: weekly scrum-of-scrums where team leads surface cross-team blockers. I own resolving cross-team blockers myself rather than leaving engineers to navigate org hierarchy alone.
>
> **Scope protection.** Business stakeholders must go through a formal change process to swap scope after PI planning. Ad-hoc additions mid-PI destroy objectives and team morale."

---

### Q12: How do you run effective retrospectives?

> "The purpose of retrospective is structural improvement, not catharsis.
>
> Three parts: what produced good outcomes (preserve), what caused friction (change), what we will do differently next sprint with specific owners and dates.
>
> The third part is the one most teams skip — vague action items with no owners. I enforce specifics: 'Add integration test for payment flow — owner: [name] — done by next sprint review.'
>
> I track retrospective action items the same way I track sprint commitments. If we are not completing retrospective actions, we are running them as therapy sessions, not improvement cycles."

---

## SECTION C — KPIs & METRICS

---

### Q13: How do you define and use KPIs for your team?

**Memory Hook:** Business → Engineering → Reliability

> "I define KPIs across three dimensions so engineering aligns with business outcomes rather than measuring activity.
>
> **Business impact** — cost reduction, time to market, customer satisfaction (NPS, adoption). Tells the business whether engineering is creating value.
>
> **Engineering efficiency** — lead time from feature sign-off to production, deployment frequency, delivery predictability. Tells me whether my processes are working.
>
> **System reliability** — SLO adherence, incident rate, MTTR. Tells me whether the systems I own are trustworthy.
>
> Balanced scorecard, not a single metric. Velocity alone can be gamed. Deployment frequency alone ignores quality. The combination is harder to game and more informative."

**Strong line:** "I use KPIs to drive outcomes, not just measure activity."

---

### Q14: Give me 3 executive-level KPIs (TWO ANSWER SETS — pick based on framing)

This question came up multiple times across interviews. Memorize **both sets** — pick based on whether the framing is **operational** (incident-focused) or **delivery-focused**.

#### **SET A — Operational / Reliability framing (Deloitte/Santosh-tested)**

**Memory Hook:** MTTD + MTTR + CFR

> "Three numbers I would present to any executive to summarize operational engineering health:
>
> **One — Mean Time to Detect (MTTD).** How long between a problem occurring and our alerting catching it. Target: under 5 minutes for P1 issues. A long MTTD means our monitoring is reactive, not proactive.
>
> **Two — Mean Time to Restore (MTTR).** How long from detection to full service restoration. Target: under 30 minutes for P1. Measures operational maturity — fast restoration means our on-call processes, runbooks, and rollback capabilities are mature.
>
> **Three — Change Failure Rate.** What percentage of production deployments cause an incident or require rollback within 24 hours. Target: below 5%. Direct measure of deployment quality and testing maturity.
>
> These three together tell the executive: how quickly we find problems, how quickly we fix them, and how well we prevent them in the first place. No technical jargon."

#### **SET B — Quality / Delivery framing (Ananth/Availity-tested)**

**Memory Hook:** Escaped Defect Rate + MTTR + Delivery Predictability

> "Three KPIs for engineering quality maturity:
>
> **One — Escaped Defect Rate to Production.** Of all defects found, what percentage were found by customers after release versus internally before release. Target: below 5%. Tells leadership whether quality gates are working — no jargon needed.
>
> **Two — Mean Time to Restore.** When something breaks, how long until customers are back to full service. Target: under 30 minutes for P1. Measures operational maturity.
>
> **Three — Delivery Predictability.** Of features committed for the quarter, what percentage shipped on the committed date. Target: above 85%. Consistently missing this means either planning is unrealistic or execution is unstable — both are signals leadership needs."

#### **Which set to use?**

| If the question is about... | Use |
|---|---|
| Operational health, on-call, incident management, reliability | **Set A** (MTTD + MTTR + CFR) |
| Quality maturity, delivery, product engineering health | **Set B** (Escaped Defect + MTTR + Predictability) |
| Generic "executive KPIs" with no framing | **Set A** — DORA-aligned, more universally recognized |

**Critical:** never say "give me time to think" on this question. You did at Deloitte and Santosh noted it. Both sets are now memorized.

---

### Q15: How do you measure productivity and performance?

**Memory Hook:** Outcome > Activity, Trends Over Points

> "I focus on outcomes, not activity metrics.
>
> Sprint velocity tells me something changed — up or down. It is a signal, not a verdict. A team completing 50 points of low-value work is less productive than one completing 30 points of high-impact work.
>
> For individual performance: delivery reliability (do they hit commitments?), output quality (defect rate, rework rate, code review feedback), ownership behavior (do they flag risks early? help unblock peers?).
>
> Combine quantitative metrics with qualitative observations. Numbers without context produce gaming. Context without numbers produces subjectivity. Both together produce accurate assessment.
>
> Track trends over at least two sprint cycles before drawing conclusions. One missed sprint is noise. A pattern is signal."

---

### Q16: How do you measure ROI of engineering initiatives?

**Memory Hook:** Cost → Speed → Value

> "Three levels.
>
> **Cost** — for infrastructure investments, establish a baseline before the initiative and measure delta after. The OpenShift to Kubernetes migration saved $5M annually — directly measurable.
>
> **Speed** — for process or tooling investments, measure time to market. If implementing a feature of known complexity previously cost X hours and now costs 0.8X, that is 20% ROI — equivalent to additional capacity within the same budget.
>
> **Business value** — for product features, track the stated business outcome. Did the feature reduce care-manager time per patient? Increase risk score accuracy? Reduce readmission rates? Engineering ROI is only complete when business outcome is measured, not just delivery.
>
> Present all three to leadership. Infrastructure savings land immediately. Speed improvements land quarterly. Business outcomes land in 2–4 quarters."

**Strong line:** "I translate engineering improvements into measurable business value."

---

### Q17: How do you prevent KPI gaming?

**Memory Hook:** Balance → Cross-Validate → Trends

> "Single metrics get gamed. Balanced scorecards are much harder to game.
>
> If I measure velocity alone, engineers decompose stories into tiny pieces to inflate count. Add quality metrics alongside velocity, and inflating velocity at quality cost becomes visible.
>
> Cross-validate across dimensions. Velocity up + defect rate up = velocity number is suspect. Deployment frequency up + MTTR up = releasing too fast without testing.
>
> Focus on trends over at least two quarters. Point-in-time numbers are easy to manipulate. Trends are not.
>
> And prioritize outcomes over activity. Features shipped per sprint is activity. Customer adoption of those features is outcome. Outcomes matter."

---

### Q18: KPIs for system reliability specifically

**Memory Hook:** Availability → Stability → Performance

| Dimension | Metric | Target |
|-----------|--------|--------|
| Availability | Uptime, service availability | >99.9% (>95% SLA at Cerner) |
| Stability | Incident rate, MTTR | P1 MTTR < 30 min |
| Performance | p50 / p95 / p99 latency | API p95 < 200ms |

> "Track SLO adherence, MTTR, and trend analysis to identify recurring issues. Focus on proactive reliability improvements — moving from incident handling to incident prevention."

---

### Q19: How do you align KPIs with business goals?

**Memory Hook:** Align → Map → Review

> "Three practices.
>
> Align KPIs with product and business priorities — not engineering preference.
>
> Map engineering metrics to business outcomes — every metric should answer 'so what does this mean for the business?'
>
> Review KPIs with stakeholders quarterly — adjust based on changing priorities. KPIs that were right 12 months ago may not be right today."

---

## QUICK REFERENCE — NUMBERS TO KNOW COLD

| Metric | Your Target/Reality |
|--------|---------------------|
| Code coverage threshold | 90% minimum, enforced in CI |
| Sprint velocity improvement after AI tools | ~20% (resume number) |
| P95 API latency target | < 200ms for query APIs |
| P1 MTTD target | Under 5 minutes |
| P1 MTTR target | Under 30 minutes |
| Change Failure Rate target | Below 5% |
| Delivery predictability target | Above 85% |
| Escaped Defect Rate target | Below 5% |
| Tech debt allocation | 20% of every sprint |
| Platform-health sprint | Once per quarter |
| SLO/SLA at Cerner | Maintained >95% |
| OpenShift → Kubernetes savings | $5M annual |
| BoA fraud analytics savings | ~$10M annual, 20% fraud reduction |
| BoA decision engine | 20–40% credit loss reduction |
| Care Coordination products | 120+ customers, $20M+ annual revenue |
| Patient records | 500M+ across customers |
| Cerner team size | 14 engineers across 2 scrum teams |
| Optum team size range | 12–20 engineers |
| Promotions enabled | 8 at Optum, 2 at Cerner |
| PIPs handled | 2 at Optum (both resolved within plan) |
| Cerner V3 model accuracy | ~40% improvement vs rule-based |

---

*File 3 of 7 — Delivery, Execution & KPIs*
