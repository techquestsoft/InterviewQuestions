# FILE 3 OF 8 — DELIVERY, EXECUTION & KPIs

> **Rule 1:** Lead with outcomes, not process. Numbers before frameworks.
> **Rule 2:** Executive KPI questions need 3 clean metrics — no jargon. Two sets memorized below (Q18).
> **Rule 3:** Spillovers, sprint discipline, tech debt — every answer needs a real example.

**Structure of every answer:** Memory Hook → Core Answer (framework) → Example (specific scene with numbers, alternate scenario, or discipline rule).

---

## CROSS-FILE INDEX

This file owns: delivery management, SAFe, sprint discipline, tech debt, KPIs, productivity measurement.
- People-side accountability → File 02
- Production reliability KPIs (MTTR/MTTD context) → File 06
- AI productivity ROI → File 07

---

# SECTION A — DELIVERY & EXECUTION

---

## Q1: How do you manage delivery end-to-end?

**Memory Hook:** Plan → Execute → Risk → Predictability

> **Core Answer**
>
> Three integrated practices.
>
> **Planning.** I align the roadmap with product priorities every quarter, balancing feature work, tech debt, and compliance items. I do not let product own the entire backlog — **deferred tech debt compounds into incidents**.
>
> **Execution discipline.** I break work into user stories small enough to complete in one sprint. I use burndown charts mid-sprint, not as a lagging indicator but as an early warning. If on day 5 we have closed only 20% of stories, I convene a triage immediately.
>
> **Risk management.** I identify dependencies early — internal and external — and track them weekly. Cross-team blockers I own and escalate myself. I do not leave engineers navigating the org alone.
>
> **Example**
>
> The outcome is delivery predictability — not every sprint is perfect, but **stakeholders never get surprised by delays**. Surprises are the failure, not slippage itself.

---

## Q2: What is RAID tracking and how do you use it?

**Memory Hook:** Risks → Assumptions → Issues → Dependencies

> **Core Answer**
>
> RAID tracking — **Risks, Assumptions, Issues, Dependencies** — is one of the primary execution governance mechanisms I use for large engineering programs. It provides structured visibility so problems are identified early rather than surfacing late in delivery.
>
> **Risks** — things that could go wrong; tracked with probability, impact, owner, mitigation.
> **Assumptions** — what we're treating as true; flagged for validation. Hidden assumptions are a common cause of slippage.
> **Issues** — things that have already gone wrong; tracked with owner and resolution date.
> **Dependencies** — what we need from other teams or vendors, and what they need from us; tracked weekly.
>
> **Example**
>
> During a multi-stream modernization initiative involving cloud migration, ML transformation, and security rollout, RAID tracking identified cross-team dependencies early. **Most enterprise slippages happen at integration points between teams** — RAID makes those visible before they become blockers.

---

## Q3: How do you ensure delivery predictability?

**Memory Hook:** Right-Size → Track Mid-Sprint → 20% Debt Capacity

> **Core Answer**
>
> Three things drive predictability.
>
> **Right-sized work** — stories too large are the primary cause of spillovers. I enforce decomposition so nothing spans more than one sprint. If a story requires two sprints, it gets broken into two stories.
>
> **Track planned vs actual weekly** — I do not wait for retrospective to find out we missed. If burndown is flat on day 6, I ask what is blocking — dependency, unclear requirement, underestimate — and resolve it.
>
> **Balance feature and tech work** — teams carrying heavy debt deliver unpredictably because every feature takes longer than estimated. I reserve **20% of sprint capacity for debt**. That 20% pays back as fewer mid-sprint surprises.
>
> **Example**
>
> Teams that protect the 20% consistently hit >85% delivery predictability. Teams that defer all debt to "next quarter" rarely do.

---

## Q4: How do you prioritize work across competing demands?

**Memory Hook:** Production → Features → Platform → Value Over Volume

> **Core Answer**
>
> My hierarchy: **production stability and customer impact first, then committed feature work, then tech debt and platform investments.**
>
> Within features, I use **business impact as the ranking criterion — not loudness of the requestor.** I translate feature requests into business value (revenue impact, customer satisfaction, compliance risk) and rank accordingly.
>
> Trade-offs presented explicitly: if a compliance item requires pushing a feature one sprint, I show the cost of *not* doing the compliance item — regulatory risk, audit exposure — alongside the cost of the delay. The business makes an informed call.
>
> **Example**
>
> What I avoid: letting urgency substitute for importance. **The loudest request is not always the most valuable one.**

---

## Q5: How do you handle sprint spillovers?

**Memory Hook:** 3 Causes, 3 Responses, Burndown Mid-Sprint

> **Core Answer**
>
> Three causes, three responses.
>
> **Underestimated complexity** — most common in legacy code. When a developer discovers it mid-sprint, we do an immediate scope triage: keep the main flow this sprint, move alternate flows to next sprint explicitly. Documented in Jira, not hidden.
>
> **External dependency blocked** — move the story back to backlog immediately. Do not carry blocked work as in-progress; it distorts the burndown.
>
> **Wrong estimate** — retrospective item. Discuss why we misjudged, update calibration for similar complexity. No blame — just calibration.
>
> I use **burndown mid-sprint, not just at end-of-sprint**. If on day 5 of 10 we have closed only 20% of stories, I triage immediately. I do not wait for retro.
>
> **Example**
>
> At Cerner, spillovers happened due to legacy code complexity — V1/V2 had been built over multiple years with patterns that surprised newer engineers. We addressed it through **deeper grooming with senior engineers walking newer ones through the legacy patterns before sprint planning**. Spillovers dropped significantly within two sprints.

---

## Q6: How do you manage releases?

**Memory Hook:** Approve → Validate → Deploy → Monitor

> **Core Answer**
>
> Four stages.
>
> **Approval** — CAB (Change Approval Board). Submit a Remedy change request with test evidence, rollback plan, and blast-radius assessment. Without CAB approval, nothing goes to production.
>
> **Validation** — all automated gates pass: code coverage, security scan, integration tests. Manual QA for edge cases that cannot be automated. **Release readiness is binary, not 'mostly ready.'**
>
> **Deployment** — Spinnaker multi-stage pipeline. For major releases, **blue-green deployment** — two live environments, cut traffic over after validation. Rollback is instant. For incremental changes, **canary** — deploy to subset of clients, monitor, expand.
>
> **Monitoring** — I do not consider a release done at deployment. I watch dashboards for **24 hours post-release**: error rates, latency, throughput. If something spikes, we roll back rather than investigating in production.
>
> **Example**
>
> **V3 ML model rollout at Cerner** used canary — deployed to 10% of clients, validated prediction accuracy matched baseline, then expanded to 100%. **Zero accuracy regression.**

---

## Q7: Walk me through the actual release management chain at Oracle Cerner

**Memory Hook:** OHRM → HDI CAB → Remedy CR → JFORMs → TTP

> **Core Answer**
>
> Five formal gates between code-complete and production.
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
> **Example**
>
> Beyond release-time governance, ongoing quality is enforced through **monthly Dev and Ops Quality Reviews, yearly internal and external audits, Solution Record Reviews, and Ops Maturity Assessments**. The discipline can feel heavy, but it's the right level of governance for healthcare. **The cost of a production incident in clinical decision support is regulatory, not just operational.**

---

## Q8: How do you balance execution and quality?

**Memory Hook:** Built-In Gates → Code Review → Demo → Feedback Loop

> **Core Answer**
>
> Quality is not a phase at the end of delivery — it is built into every step.
>
> **In the pipeline:** 90% code coverage enforced, SonarQube quality gates, Fortify security scanning. Not suggestions — blockers. Code that fails does not progress.
>
> **In code review:** I am the mandatory second-level approver. I check architectural conformance, boundary violations, exception handling. Not formatting — automated tools handle that.
>
> **In sprint review:** I check demo quality. If the demo does not reflect what was committed, that is a quality signal even if code passes all automated gates.
>
> **Feedback loop:** production incidents go back into the backlog as corrective actions. **Every incident produces at least one structural fix, not just a workaround.**

---
## Q9: What do you do if quality drops while scaling?

**Memory Hook:** Clarify Which Scaling → Diagnose → Tighten Gates → Invest in Maturity

> **Discipline Rule**
>
> "Scaling" is ambiguous in this question. **Briefly clarify** what kind of scaling — team, system, or delivery velocity — because the diagnosis and fix differ for each.

> **Core Answer**
>
> Three flavors of scaling, each with a different quality-drop pattern.
>
> **Team scaling (more engineers).** Quality drops because **standards don't propagate to new hires automatically**. Symptoms: defects clustered around recent joiners or newly formed teams. Fixes: structured onboarding with paired reviews for the first month, explicit coding standards in CI gates rather than in tribal knowledge, **senior engineer ratio protected** when expanding (you can't dilute past a threshold without quality cost).
>
> **System scaling (more load, more customers).** Quality drops because **edge cases that didn't matter at small scale now surface**. Symptoms: incidents on hot paths, race conditions, performance regressions under load. Fixes: load testing as a gate before major customer onboarding, observability on p99 and tail latency rather than averages, and **chaos engineering** for failure modes you can't predict.
>
> **Delivery scaling (shipping faster, parallel sprints).** Quality drops because **pressure compresses review and testing time**, and tech debt accumulates. Symptoms: spike in escaped defects, recurring incidents in the same areas, rising rework rate. Fixes: tighten release gates — **a release introducing known P1/P2 defects doesn't go out, period** — enforce coverage thresholds, mandate design reviews, and protect the 20% tech-debt allocation.
>
> **Common thread across all three**
>
> First question is always: **is this individual or systemic?** If multiple engineers or services are showing the same defect pattern, it's a system problem — unclear requirements, missing standards, or compressed review. Fix the system before coaching individuals.
>
> **Defect rates are lagging indicators of process and skill gaps. Fix the gap, not just the symptom.**

---

## Q10: How do you manage technical debt?

**Memory Hook:** 20% Rule → Visible Register → WSJF(Weighted Shortest Job First) → Platform Health Sprints

> **Core Answer**
>
> Tech debt is a first-class backlog item, not a conversation that happens when there is spare capacity (which there never is).
>
> **The 20% rule.** I reserve 20% of every sprint for debt, refactoring, and non-functional improvements. Non-negotiable. When stakeholders push back, I use the compound-interest analogy: **debt not addressed today costs three times more in six months.**
>
> **Visible register.** Tech debt register with four fields per item: estimated cost of delay, risk rating, owner, effort estimate. Visible to stakeholders. When someone asks why a feature is slower than expected, I show them the debt we are carrying.
>
> **WSJF prioritization** — Weighted Shortest Job First. High-risk, low-effort items first. The most dangerous debt gets resolved without requiring long dedicated sprints.
>
> **Platform-health sprints.** For significant debt requiring sustained focus, I negotiate one dedicated sprint per quarter. No feature work — debt, performance, observability only.
>
> **Example**
>
> At Cerner, a **legacy Oracle integration was causing 40% of production incidents from one layer**. One platform-health sprint per quarter, over two quarters, **dropped the incident rate from that layer by 60%**.

---

## Q11: How do you balance modernization work with business delivery commitments?

**Memory Hook:** Incremental → Risk-Based → Business-Aligned

> **Core Answer**
>
> I do not treat modernization and business delivery as competing priorities. I focus on **phased modernization** approaches that improve scalability, resiliency, or engineering efficiency while still maintaining predictable customer delivery.
>
> Avoid big-bang rewrites. Sequence work so each phase delivers value independently and reduces risk for the next phase. Run modernization alongside business commitments rather than in dedicated cutover windows whenever possible.
>
> **Example**
>
> During cloud modernization, we balanced **AWS-to-OCI migration and operational improvements alongside active client commitments** by sequencing service-by-service, running both environments in parallel during cutover, and avoiding any large disruptive rewrite. Zero customer-impacting outages during the migration.

---

## Q12: How do you handle multiple competing initiatives?

**Memory Hook:** Split → Named Owners → Weekly Integration Sync

> **Core Answer**
>
> **Workstream-based management.**
>
> Break the portfolio into clearly bounded workstreams. Each workstream gets a **named owner — not me**. I become the program-level integrator, not the bottleneck.
>
> Track each workstream against its objectives weekly. The **integration points between workstreams get explicit attention — that is where things break.**
>
> Stakeholder communication happens at program level, not per-workstream. One coherent narrative to leadership, not three competing ones.
>
> **Example**
>
> At Cerner, I was simultaneously running **V3 ML transformation, AWS-to-OCI cloud migration, and CrowdStrike security rollout**. Three workstreams, three named owners. I ran a **weekly 30-minute integration sync** — that single ceremony **surfaced ~80% of the cross-workstream conflicts before they became blockers**.

---

## Q13: How do you manage large cross-functional engineering programs?

**Memory Hook:** Structure → Ownership → Governance → Visibility

> **Core Answer**
>
> Four pillars.
>
> **Structure** — decompose into workstreams with clear scope, dependencies, and integration points.
>
> **Ownership** — each workstream has a named lead accountable for delivery. I am program-level, not workstream-level.
>
> **Governance** — structured weekly reviews covering progress, risks (RAID), and dependency unblocking. Monthly architecture reviews to keep workstreams aligned.
>
> **Visibility** — single program dashboard accessible to stakeholders. Status, risks, decisions needed — all in one place. **Most enterprise delivery issues happen at integration points between teams, so dependency management is the single highest-leverage activity.**
>
> **Example**
>
> At Cerner, managing parallel initiatives across cloud migration, ML modernization, and security rollout — structured governance reviews and weekly dependency tracking maintained delivery predictability without compromising operational stability.

---

## Q14: How do you align delivery with business outcomes?

**Memory Hook:** Map → Track → Communicate

> **Core Answer**
>
> **Map every engineering investment to a business outcome at the start.** Not "build feature X" — *"reduce care manager time per patient by 15% so they can serve 20% more patients."* Engineering work without a stated business outcome is hard to justify and easy to deprioritize.
>
> **Track outcome metrics, not just delivery metrics.** Did we ship the feature on time? Yes — but did adoption hit the predicted level? Did the business outcome materialize within 1 to 2 quarters of release?
>
> **Communicate outcomes back to engineering.** Engineers who see the business impact of their work stay engaged. Engineers who only see story-point throughput burn out faster.
>
> **Example**
>
> Care Coordination products at Cerner generated **$20M+ annual revenue** serving 120+ customers — every feature was tied to a measurable clinical or operational outcome (readmission reduction, care-manager productivity, risk score accuracy).

---

# SECTION B — AGILE & DELIVERY FRAMEWORKS

---

## Q15: How do you scale delivery using SAFe?

**Memory Hook:** PI Pre-Work → Dual Tracking → Own Dependencies → Protect Scope

> **Core Answer**
>
> SAFe works well when multiple teams need to coordinate toward a shared product increment. The failure modes are well-known — I prevent them specifically.
>
> **PI Planning quality.** The ceremony is only as good as the pre-work. Teams must enter with refined, estimated backlog items — not vague stories. I enforce a refinement checkpoint **two weeks before PI planning**.
>
> **Dual tracking.** I track at team level (sprint velocity, commitment accuracy) and program level (PI objective completion rate). If program completion drops below 80%, I investigate whether it is a planning failure or execution failure — different causes, different fixes.
>
> **Dependency management** — the most common SAFe failure. Weekly scrum-of-scrums where team leads surface cross-team blockers. **I own resolving cross-team blockers myself** rather than leaving engineers to navigate org hierarchy alone.
>
> **Scope protection.** Business stakeholders must go through a formal change process to swap scope after PI planning. **Ad-hoc additions mid-PI destroy objectives and team morale.**

---

## Q16: How do you run effective retrospectives?

**Memory Hook:** Preserve → Change → Specific Owners and Dates

> **Core Answer**
>
> The purpose of retrospective is **structural improvement, not catharsis** (emotional release).
>
> Three parts: what produced good outcomes (preserve), what caused friction (change), what we will do differently next sprint **with specific owners and dates**.
>
> The third part is the one most teams skip — vague action items with no owners. I enforce specifics: *"Add integration test for payment flow — owner: [name] — done by next sprint review."*
>
> **Example**
>
> I track retrospective action items the same way I track sprint commitments. **If we are not completing retrospective actions, we are running them as therapy sessions, not improvement cycles.**

---

# SECTION C — KPIs & METRICS

---

## Q17: How do you define and use KPIs for your team?

**Memory Hook:** Business → Engineering → Reliability (Three Dimensions, Balanced)

> **Core Answer**
>
> I define KPIs across **three dimensions** so engineering aligns with business outcomes rather than measuring activity.
>
> **Business impact** — for example, reducing cloud infrastructure cost by 15%, improving feature adoption by 20%, or improving customer onboarding completion. Tells the business whether engineering is creating value.
>
> **Engineering efficiency** — lead time for changes (commit to production), deployment frequency, and delivery predictability. Example goals for a regulated healthcare environment: reducing lead time from two weeks to one week through better automation in the pre-CAB stages; increasing deployment frequency from bi-weekly to weekly with feature flags decoupling deployment from release; improving sprint predictability from 70% to 90% through better story decomposition. These metrics tell me whether my engineering processes are working.
>
> **System reliability and quality** — SLO adherence, incident rate, MTTR, and defect escape rate. Example goal: **reducing defect escape rate below 2% for the features my team owns this quarter.** Tells me whether systems and engineering practices are truly production-ready.
>
> **Balanced scorecard, not a single metric.** Velocity alone can be gamed. Deployment frequency alone ignores quality. The combination is harder to game and more informative.
>
> **Example — SMART KPI**
>
> *"Reduce API response time from 800ms to under 300ms for 95% of requests by the end of Q3."*
>
> - **Specific** → API response time
> - **Measurable** → 800ms → 300ms
> - **Achievable** → realistic target
> - **Relevant** → improves user experience
> - **Time-bound** → end of Q3
>
> **Strong close:** "I use KPIs to drive outcomes, not just measure activity."

---

## Q18: Give me 3 executive-level KPIs

**Memory Hook:** TWO SETS — Operational (Set A) or Quality/Delivery (Set B)

> **Discipline Rule**
>
> This question came up multiple times across interviews. **Memorize both sets.** Pick based on whether the framing is operational (incident-focused) or delivery-focused. **Never say "give me time to think"** — that hurt the Deloitte interview.

> ### SET A — Operational / Reliability framing (Deloitte/Santosh-tested)
>
> **Memory Hook:** MTTD + MTTR + CFR
>
> Three numbers I would present to any executive to summarize operational engineering health:
>
> **One — Mean Time to Detect (MTTD).** How long between a problem occurring and our alerting catching it. **Target: under 5 minutes for P1.** Long MTTD means monitoring is reactive, not proactive.
>
> **Two — Mean Time to Restore (MTTR).** How long from detection to full service restoration. **Target: under 30 minutes for P1.** Measures operational maturity — fast restoration means on-call processes, runbooks, and rollback capabilities are mature.
>
> **Three — Change Failure Rate (CFR).** Percentage of production deployments that cause an incident or require rollback within 24 hours. **Target: below 5%.** Direct measure of deployment quality and testing maturity.
>
> These three together tell the executive: **how quickly we find problems, how quickly we fix them, and how well we prevent them in the first place.** No technical jargon.

> ### SET B — Quality / Delivery framing (Ananth/Availity-tested)
>
> **Memory Hook:** Escaped Defect Rate + MTTR + Delivery Predictability
>
> Three KPIs for engineering quality maturity:
>
> **One — Escaped Defect Rate to Production.** Of all defects found, what percentage were found by customers after release versus internally before release. **Target: below 5%.** Tells leadership whether quality gates are working.
>
> **Two — Mean Time to Restore.** When something breaks, how long until customers are back to full service. **Target: under 30 minutes for P1.** Measures operational maturity.
>
> **Three — Delivery Predictability.** Of features committed for the quarter, what percentage shipped on the committed date. **Target: above 85%.** Consistently missing this means either planning is unrealistic or execution is unstable — both are signals leadership needs.

> **Which set to use?**
>
> | If the question is about... | Use |
> |---|---|
> | Operational health, on-call, incident management, reliability | **Set A** (MTTD + MTTR + CFR) |
> | Quality maturity, delivery, product engineering health | **Set B** (Escaped Defect + MTTR + Predictability) |
> | Generic "executive KPIs" with no framing | **Set A** — DORA-aligned, more universally recognized |

---

## Q19: How do you measure productivity and performance?

**Memory Hook:** Outcome > Activity → Trends Over Points

> **Core Answer**
>
> I focus on **outcomes, not activity metrics.**
>
> Sprint velocity tells me something changed — up or down. **It is a signal, not a verdict.** A team completing 50 points of low-value work is less productive than one completing 30 points of high-impact work.
>
> For **individual performance**: delivery reliability (do they hit commitments?), output quality (defect rate, rework rate, code review feedback), ownership behavior (do they flag risks early? help unblock peers?).
>
> **Combine quantitative metrics with qualitative observations.** Numbers without context produce gaming. Context without numbers produces subjectivity. Both together produce accurate assessment.
>
> **Track trends over at least two sprint cycles before drawing conclusions.** One missed sprint is noise. A pattern is signal.

---

## Q20: How do you measure ROI of engineering initiatives?

**Memory Hook:** Cost → Speed → Business Value

> **Core Answer**
>
> Three levels.
>
> **Cost** — for infrastructure investments, establish a baseline before the initiative and measure delta after. **The OpenShift to Kubernetes migration saved $5M annually** — directly measurable.
>
> **Speed** — for process or tooling investments, measure time to market. If implementing a feature of known complexity previously cost X hours and now costs 0.8X, that is 20% ROI — equivalent to additional capacity within the same budget.
>
> **Business value** — for product features, track the stated business outcome. Did the feature reduce care-manager time per patient? Increase risk score accuracy? Reduce readmission rates? **Engineering ROI is only complete when business outcome is measured, not just delivery.**
>
> **Example**
>
> Present all three to leadership. Infrastructure savings land immediately. Speed improvements land quarterly. Business outcomes land in 2–4 quarters.
>
> **Strong close:** "I translate engineering improvements into measurable business value."

---

## Q21: How do you prevent KPI gaming?

**Memory Hook:** Balance → Cross-Validate → Trends → Outcomes Over Activity

> **Core Answer**
>
> **Single metrics get gamed. Balanced scorecards are much harder to game.**
>
> If I measure velocity alone, engineers decompose stories into tiny pieces to inflate count. Add quality metrics alongside velocity, and inflating velocity at quality cost becomes visible.
>
> **Cross-validate across dimensions.** Velocity up + defect rate up = velocity number is suspect. Deployment frequency up + MTTR up = releasing too fast without testing.
>
> **Focus on trends over at least two quarters.** Point-in-time numbers are easy to manipulate. Trends are not.
>
> **Prioritize outcomes over activity.** Features shipped per sprint is activity. Customer adoption of those features is outcome. **Outcomes matter.**

---

## Q22: KPIs for system reliability specifically

**Memory Hook:** Availability → Stability → Performance

> **Core Answer**
>
> Three dimensions, with concrete targets.
>
> | Dimension | Metric | Target |
> |-----------|--------|--------|
> | **Availability** | Uptime, service availability | >99.9% (>95% SLA at Cerner) |
> | **Stability** | Incident rate, MTTR | P1 MTTR < 30 min |
> | **Performance** | p50 / p95 / p99 latency | API p95 < 200ms |
>
> Track SLO adherence, MTTR, and trend analysis to identify recurring issues. **Focus on proactive reliability — moving from incident handling to incident prevention.**
>
> **Example**
>
> At Cerner, we maintained **>95% SLO/SLA** across Care Coordination products serving 120+ customers, with sustained P1 MTTR under 30 minutes through mature on-call processes and runbooks.

---

## Q23: How do you align KPIs with business goals?

**Memory Hook:** Align → Map → Review Quarterly

> **Core Answer**
>
> Three practices.
>
> **Align** KPIs with product and business priorities — not engineering preference.
>
> **Map** engineering metrics to business outcomes — every metric should answer *"so what does this mean for the business?"*
>
> **Review** KPIs with stakeholders quarterly — adjust based on changing priorities. **KPIs that were right 12 months ago may not be right today.**
>
> **Example**
>
> When the business priority shifts from "ship faster" to "improve stability," the KPI portfolio must shift too — otherwise engineering is optimizing for last quarter's goals while leadership is asking about this quarter's.

---

# QUICK REFERENCE — MEMORY HOOKS

| # | Topic | Memory Hook |
|---|---|---|
| Q1 | Manage delivery end-to-end | Plan → Execute → Risk → Predictability |
| Q2 | RAID tracking | Risks → Assumptions → Issues → Dependencies |
| Q3 | Delivery predictability | Right-Size → Track Mid-Sprint → 20% Debt Capacity |
| Q4 | Prioritize work | Production → Features → Platform → Value Over Volume |
| Q5 | Sprint spillovers | 3 Causes, 3 Responses, Burndown Mid-Sprint |
| Q6 | Manage releases | Approve → Validate → Deploy → Monitor (24h) |
| Q7 | Oracle release chain | OHRM → HDI CAB → Remedy CR → JFORMs → TTP |
| Q8 | Execution vs quality | Built-In Gates → Code Review → Demo → Feedback Loop |
| Q9 | Quality drops while scaling | Diagnose Systemic → Tighten → Invest in Maturity |
| Q10 | Technical debt | 20% Rule → Register → WSJF → Platform-Health Sprints |
| Q11 | Modernization vs delivery | Incremental → Risk-Based → Business-Aligned |
| Q12 | Multiple initiatives | Split → Named Owners → Weekly Integration Sync |
| Q13 | Large cross-functional programs | Structure → Ownership → Governance → Visibility |
| Q14 | Delivery to business outcomes | Map → Track → Communicate |
| Q15 | SAFe | PI Pre-Work → Dual Track → Own Deps → Protect Scope |
| Q16 | Retrospectives | Preserve → Change → Specific Owners & Dates |
| Q17 | Team KPIs | Business → Engineering → Reliability (Balanced) |
| Q18 | 3 executive KPIs | **Set A**: MTTD + MTTR + CFR / **Set B**: Escaped Defect + MTTR + Predictability |
| Q19 | Productivity & performance | Outcome > Activity → Trends Over Points |
| Q20 | ROI of initiatives | Cost → Speed → Business Value |
| Q21 | Prevent KPI gaming | Balance → Cross-Validate → Trends → Outcomes |
| Q22 | Reliability KPIs | Availability → Stability → Performance |
| Q23 | Align KPIs to business | Align → Map → Review Quarterly |

---

# APPENDIX A — CONSOLIDATED KPI REFERENCE

> *Reference material for senior EM / Engineering Director-level KPI discussions. Use this to pick the right metrics for the question being asked.*

---

## Master KPI Table

| KPI | Engineering | Business | Executive | Quality Maturity | Reliability | Delivery / Efficiency | Typical Target | Owner |
|---|:---:|:---:|:---:|:---:|:---:|:---:|---|---|
| **Lead Time for Changes** (commit → prod) | ✅ | | ✅ | | | ✅ | < 1 hr (elite) / < 1 day (high) | Engineering |
| **Deployment Frequency** | ✅ | | ✅ | | | ✅ | Daily or on-demand | Engineering |
| **Delivery Predictability** | ✅ | | ✅ | ✅ | | ✅ | ≥ 85% | Engineering |
| **Change Failure Rate** | ✅ | | ✅ | ✅ | ✅ | | < 5% | Engineering |
| **Defect Escape Rate** | ✅ | | | ✅ | | | < 2–5% | Engineering / QA |
| **MTTD** | ✅ | | ✅ | | ✅ | | < 5 min (P1) | SRE / Engineering |
| **MTTR** | ✅ | | ✅ | | ✅ | | < 30 min (P1) | SRE / Engineering |
| **SLO Adherence** | ✅ | | ✅ | ✅ | ✅ | | ≥ 99.9% (customer APIs) | SRE / Engineering |
| **Incident Rate** (P1/P2 count) | ✅ | | | | ✅ | | Trending down QoQ | SRE / Engineering |
| **Cloud Cost Reduction** | | ✅ | ✅ | | | | -15% YoY (illustrative) | Engineering + Finance |
| **Feature Adoption Rate** | | ✅ | ✅ | | | | +20% per quarter (illustrative) | Product (Eng contributes) |
| **Customer Onboarding Completion Rate** | | ✅ | ✅ | | | | Domain-dependent | Product (Eng contributes) |

A KPI may appear under multiple lenses — that's expected; the same metric can serve engineering, executives, and quality maturity simultaneously.

---

## Which KPIs to Pull by Interview Question

| Interview Question | KPIs to Pull |
|---|---|
| "How do you set goals for your engineering team?" | 1 business + 2 delivery/efficiency + 2 reliability/quality |
| "What three metrics would you present to an executive?" | **Set A:** MTTD + MTTR + Change Failure Rate |
| "How do you measure engineering quality maturity?" | **Set B:** Defect Escape Rate + MTTR + Delivery Predictability |
| "How do you measure delivery throughput?" | The four DORA: Lead Time, Deployment Frequency, CFR, MTTR |
| "How does engineering create business value?" | Cost reduction, feature adoption, onboarding completion — engineering *contributes*, doesn't *own* |

---

## Three-Dimension KPI Framework (goal-setting answers)

| Dimension | Purpose | Example KPIs | Example Quarterly Goal |
|---|---|---|---|
| **Business Impact** | Whether engineering is creating value | Cloud cost reduction, feature adoption, onboarding completion | Reduce cloud cost by 15% |
| **Engineering Efficiency** | Whether engineering processes are working | Lead Time, Deployment Frequency, Delivery Predictability | Reduce lead time from 4 hours to 1 hour |
| **System Reliability & Quality** | Whether systems are production-ready | SLO Adherence, Incident Rate, MTTR, Defect Escape Rate | Defect escape rate below 2% |

---

## DORA Metrics — The Engineering Core Four

| DORA Metric | Measures | Elite Target | High Target |
|---|---|---|---|
| **Lead Time for Changes** | Speed | < 1 hour | < 1 day |
| **Deployment Frequency** | Speed | On-demand / multiple per day | Daily to weekly |
| **Change Failure Rate** | Stability | 0–15% | 16–30% |
| **MTTR** | Stability | < 1 hour | < 1 day |

**Speed metrics without stability metrics is a red flag — and vice versa.** The pairing is the point.

---

## Engineering KPI vs Business KPI — Ownership Distinction

| Aspect | Engineering KPI | Business KPI |
|---|---|---|
| **Who owns it** | Engineering, fully | Product / Business, with engineering contributing |
| **What it measures** | Engineering function health | Business outcome |
| **Example** | Deployment Frequency | Feature Adoption Rate |
| **Accountability** | Engineering hits or misses alone | Shared across product, engineering, GTM |
| **Used for** | Team performance, process improvement | Strategic prioritization, ROI |

A team can have great DORA metrics and still fail the business. A team can hit business numbers while accumulating engineering debt. **Track both, but don't conflate them.**

---

## The Balanced Scorecard Principle

Single metrics get gamed:

- **Velocity alone** → teams inflate story points
- **Deployment frequency alone** → teams ship junk to hit the number
- **Defect rate alone** → teams under-report or over-classify
- **MTTR alone** → teams declare incidents resolved prematurely

The combination is harder to game and more informative. **A senior engineering leader presents a portfolio of metrics, not a single dashboard number.**

---

# APPENDIX B — NUMBERS TO KNOW COLD

| Metric | Your Target / Reality |
|--------|----------------------|
| Code coverage threshold | 90% minimum, enforced in CI |
| Sprint velocity improvement after AI tools | ~20% (resume number) |
| P95 API latency target | < 200ms for query APIs |
| P1 MTTD target | Under 5 minutes |
| P1 MTTR target | Under 30 minutes |
| Change Failure Rate target | Below 5% |
| Delivery predictability target | Above 85% |
| Escaped Defect Rate target | Below 5% (Set B); below 2% (stretch) |
| Tech debt allocation | 20% of every sprint |
| Platform-health sprint cadence | Once per quarter |
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
| Cerner legacy Oracle layer incident reduction | 60% across two platform-health sprints |
| Cerner cross-workstream conflict catch rate | ~80% via weekly 30-min integration sync |

---

*File 3 of 8 — Delivery, Execution & KPIs (merged master)*
