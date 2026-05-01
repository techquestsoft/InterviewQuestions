# Interview Prep — File 3 of 6
# Delivery, Execution, KPIs & Agile

> **Rule:** When asked about delivery, lead with outcomes not process. Numbers before frameworks.
> **Rule:** KPI questions at executive level need 3 clean metrics max — no jargon.

---

## SECTION A — DELIVERY & EXECUTION

---

### Q1: How do you manage delivery end-to-end?

**Memory Hook:** Plan → Break → Execute → Risk → Deliver

> "I manage delivery through three integrated practices.
>
> Planning: I align the roadmap with product priorities every quarter, balancing feature work, tech debt, and compliance items. I do not let product own the entire backlog — tech debt that is deferred compounds into incidents.
>
> Execution discipline: I break work into user stories small enough to complete in one sprint. I use burndown charts mid-sprint — not as a lagging indicator but as an early warning. If we are at day 5 and have only closed 20% of stories, I convene a triage immediately.
>
> Risk management: I identify dependencies early — internal and external — and track them weekly. Cross-team blockers I own and escalate myself. I do not leave engineers navigating org hierarchy alone.
>
> The outcome is delivery predictability — not every sprint is perfect, but stakeholders never get surprised by delays."

---

### Q2: How do you ensure delivery predictability?

**Memory Hook:** Small Work → Track → Balance

> "Three things drive predictability.
>
> Right-sized work: stories that are too large are the primary cause of spillovers. I enforce story decomposition so nothing spans more than one sprint. If a story requires two sprints, it gets broken into two stories.
>
> Track planned vs actual weekly: I do not wait for the retrospective to find out we missed. If the burndown is flat on day 6, I ask what is blocking — dependency, unclear requirement, underestimate — and I resolve it.
>
> Balance feature and tech work: teams that carry heavy tech debt deliver unpredictably because every feature takes longer than estimated. I reserve 20% of sprint capacity for debt. That 20% pays back in the form of fewer mid-sprint surprises."

---

### Q3: How do you prioritize work across competing demands?

**Memory Hook:** Production → Features → Platform

> "My prioritization hierarchy is: production stability and customer impact first, then committed feature work, then tech debt and platform investments.
>
> Within features: I use business impact as the ranking criterion, not loudness of the requestor. I translate feature requests into business value — revenue impact, customer satisfaction, compliance risk — and rank accordingly.
>
> Trade-offs I present explicitly: if a compliance item requires pushing a feature one sprint, I show the cost of not doing the compliance item — regulatory risk, potential audit exposure — alongside the cost of the delay. The business makes an informed call.
>
> What I avoid: letting urgency substitute for importance. The loudest request is not always the most valuable one."

---

### Q4: How do you handle delays or risks?

**Memory Hook:** Find → Communicate → Adjust

> "When I see a risk forming, I communicate early — not when it has already become a miss.
>
> I identify the root cause: is it underestimated complexity, a blocked dependency, or scope growth? Each has a different response.
>
> If it is underestimated complexity: I do a scope triage — keep the main flow in this sprint, move alternate flows to the next sprint explicitly. Not as a failure, but as a deliberate adjustment.
>
> If it is a blocked external dependency: I move the story back to the backlog immediately rather than carrying it as false work-in-progress. Burndown should reflect reality.
>
> I communicate to stakeholders with three things: what changed, what the impact is, and what the path forward is. Never just 'we are delayed.' That is not actionable."

---

### Q5: How do you balance execution and quality together?

**Memory Hook:** Execute + Control + Feedback

> "Quality is not a phase at the end of delivery — it is built into every step.
>
> In the pipeline: 90% code coverage enforced, SonarQube quality gates, Fortify security scanning. These are not suggestions — they are blockers. Code that fails does not progress.
>
> In code review: I am the mandatory second-level approver. I check architectural conformance, boundary violations, exception handling. Not formatting — automated tools handle that.
>
> In sprint review: I check demo quality. If the demo does not reflect what was committed, that is a quality signal — even if the code passes all automated gates.
>
> The feedback loop: production incidents go back into the backlog as corrective actions. Every incident should produce at least one structural fix, not just a workaround."

---

### Q6: What do you do if quality drops while scaling?

**Memory Hook:** Control → Improve → Prevent

> "When quality drops, my first question is: is this individual or systemic? If multiple engineers are introducing defects, it is usually a system problem — unclear requirements, insufficient test coverage standards, or time pressure that compressed review.
>
> I analyze by defect type: are they test coverage gaps? Integration issues? Regressions from tech debt? Different causes need different fixes.
>
> For immediate control: I tighten release gates. A release that introduces known P1 or P2 defects does not go out — period. I make this non-negotiable even when there is business pressure.
>
> For improvement: I introduce or enforce the practices that were slipping — coverage thresholds, integration test suites, design reviews before implementation.
>
> For prevention: I invest in engineering maturity. Defect rates are a lagging indicator of process and skill gaps. I address the gap, not just the symptom."

---

### Q7: How do you handle sprint spillovers?

> "Three causes, three responses.
>
> Underestimated complexity: most common in legacy systems. When a developer discovers the code is more complex than estimated, mid-sprint we do an immediate scope triage — main flow stays, alternate flows move to next sprint explicitly. Documented in Jira, not hidden.
>
> External dependency blocked us: move the story back to the backlog immediately. Do not carry blocked work as in-progress — it distorts the burndown and creates false confidence.
>
> Wrong estimate: retrospective item. We discuss why we misjudged, update our reference points for similar complexity. No blame — just calibration.
>
> I use burndown mid-sprint, not just at end-of-sprint. If on day 5 of 10 we have closed only 20% of stories, I triage immediately — not on day 10."

---

### Q8: How do you manage releases?

**Memory Hook:** Approve → Validate → Deploy → Monitor

> "Our release process has four stages.
>
> Approval: CAB — Change Advisory Board. We submit a Remedy change request with test evidence, rollback plan, and blast radius assessment. Without CAB approval, nothing goes to production.
>
> Validation: all automated gates must pass — code coverage, security scan, integration tests. Manual QA for edge cases that cannot be automated. Release readiness is a binary — not 'mostly ready.'
>
> Deployment: we use Spinnaker with multi-stage pipeline. For major releases, blue-green deployment — two live environments, cut traffic over when validated. For incremental changes, canary — deploy to a subset of clients, monitor, then expand. Rollback is instant with blue-green.
>
> Monitoring: I do not consider a release done at deployment. I watch dashboards for 24 hours post-release — error rates, latency, throughput. If something spikes, we roll back immediately rather than investigating in production."

---

### Q9: How do you manage technical debt?

**Memory Hook:** 20% Rule → Visible Register → WSJF → Platform Health Sprints

> "Tech debt is a first-class backlog item — not a conversation that happens when there is spare capacity, which there never is.
>
> The 20% rule: I reserve 20% of every sprint for debt, refactoring, and non-functional improvements. Non-negotiable. When business stakeholders push back, I use the compound interest analogy: debt not addressed today costs three times more to fix in six months.
>
> Visible register: I maintain a tech debt register with four fields per item — estimated cost of delay, risk rating, owner, and effort. This register is visible to stakeholders. When someone asks why a feature is slower than expected, I show them the debt we are carrying.
>
> WSJF prioritization: high-risk, low-effort debt items get addressed first. This ensures the most dangerous debt is resolved without requiring long dedicated sprints.
>
> Platform health sprints: for significant debt requiring sustained focus, I negotiate one dedicated sprint per quarter. No feature work. Debt, performance, observability only.
>
> Real example: at Cerner, a legacy Oracle integration layer was causing 40% of our production incidents. One platform health sprint per quarter over two quarters dropped the incident rate from that layer by 60%."

---

## SECTION B — AGILE & DELIVERY FRAMEWORKS

---

### Q10: How do you scale delivery across multiple teams using SAFe?

**Memory Hook:** Pre-work → Dual Tracking → Own Dependencies → Protect Scope

> "SAFe works well when multiple teams need to coordinate toward a shared product increment. The failure modes are well-known — I try to prevent them specifically.
>
> PI Planning quality: the ceremony is only as good as the pre-work. Teams must enter with refined, estimated backlog items — not vague stories. I enforce a refinement checkpoint two weeks before PI planning.
>
> Dual tracking: I track at team level (sprint velocity, commitment accuracy) and program level (PI objective completion rate). If program completion drops below 80%, I investigate whether it is a planning failure or an execution failure — different causes, different fixes.
>
> Dependency management — the most common SAFe failure: weekly scrum-of-scrums where team leads surface cross-team blockers. I own resolving cross-team blockers myself rather than leaving engineers to navigate the org alone.
>
> Scope protection: business stakeholders must go through a formal change process to swap scope after PI planning. Ad hoc additions mid-PI destroy objectives and team morale."

---

### Q11: How do you run effective retrospectives?

> "The purpose of a retrospective is not catharsis — it is structural improvement.
>
> I run retrospectives in three parts: what produced good outcomes and should be preserved, what caused friction and needs to change, and what we are going to do differently next sprint with specific owners and dates.
>
> The third part is the one most teams skip — vague action items with no owners. I enforce specific actions: 'Add integration test for the payment flow — owner: [name] — done by next sprint review.'
>
> I track retrospective action items the same way I track sprint commitments. If we are not completing retrospective actions, we are running them as therapy sessions, not improvement cycles."

---

## SECTION C — KPIs & METRICS

---

### Q12: How do you define and use KPIs for your team?

**Memory Hook:** Business → Engineering → Reliability

> "I define KPIs across three dimensions so engineering aligns with business outcomes rather than just measuring activity.
>
> Business impact: cost reduction, time to market, customer satisfaction (NPS, adoption). These tell the business whether engineering is creating value.
>
> Engineering efficiency: lead time from feature sign-off to production deployment, deployment frequency, delivery predictability. These tell me whether my processes are working.
>
> System reliability: SLO adherence, incident rate, MTTR. These tell me whether the systems I own are trustworthy.
>
> I use a balanced scorecard — no single metric. Velocity alone can be gamed. Deployment frequency alone ignores quality. The combination is harder to game and more informative."

**Strong line:** "I use KPIs to drive outcomes, not just measure activity."

---

### Q13: Give me three executive-level KPIs for quality and operational health

> "Three numbers I would present to any executive to summarize engineering health:
>
> One — Escaped Defect Rate to Production: of all defects found, what percentage were found by customers after release versus found internally before release. Target below 5%. This tells the executive whether our quality gates are working, without any technical jargon.
>
> Two — Mean Time to Restore (MTTR): when something breaks, how long until customers are back to full service. Target under 30 minutes for P1 incidents. This measures our operational maturity — fast restoration means our on-call processes, runbooks, and rollback capabilities are mature.
>
> Three — Delivery Predictability: of features committed for a quarter, what percentage shipped on the committed date. Target above 85%. Consistently missing this number means either planning is unrealistic or execution is unstable — both are important signals for leadership."

---

### Q14: How do you measure productivity and performance?

**Memory Hook:** Outcome > Activity

> "I focus on outcomes, not activity metrics.
>
> Sprint velocity tells me something changed — up or down. It is a signal, not a verdict. A team that completes 50 story points of low-value work is less productive than one that completes 30 points of high-impact work.
>
> For individual performance I look at: delivery reliability (do they hit what they commit?), quality of output (defect rate, rework rate, code review feedback), and ownership behavior (do they flag risks early? do they help unblock peers?).
>
> I combine quantitative metrics with qualitative observations. Numbers without context produce gaming. Context without numbers produces subjectivity. Both together produce accurate assessment.
>
> I track trends over at least two sprint cycles before drawing conclusions. One missed sprint is noise. A pattern is signal."

---

### Q15: How do you measure ROI of engineering initiatives?

**Memory Hook:** Cost → Speed → Value

> "I measure at two levels.
>
> Cost level: for infrastructure investments, I establish a baseline cost before the initiative and measure the delta after. The OpenShift to Kubernetes migration saved $9M annually — that is directly measurable.
>
> Speed level: for process or tooling investments, I measure time to market. If implementing a feature of known complexity previously cost X hours and now costs 0.8X, that is a 20% ROI — or equivalently, additional capacity to deliver more within the same budget.
>
> Business value level: for product features, I track the stated business outcome — did the feature reduce care manager time per patient, increase risk score accuracy, reduce readmission rates? Engineering ROI is only complete when the business outcome is measured, not just the delivery.
>
> I present all three to leadership. Infrastructure savings land immediately. Speed improvements land quarterly. Business outcomes land in 2 to 4 quarters."

---

### Q16: How do you prevent KPI gaming?

**Memory Hook:** Balance → Validate → Trend

> "Single metrics get gamed. Balanced scorecards are much harder to game.
>
> If I measure velocity alone, engineers decompose stories into tiny pieces to inflate count. If I add quality metrics alongside velocity, suddenly inflating velocity at the cost of quality is visible and penalized.
>
> I cross-validate across dimensions. If velocity is up but defect rate is also up, the velocity number is suspect. If deployment frequency is up but MTTR is also up, we are releasing too fast without proper testing.
>
> I focus on trends over at least two quarters. Point-in-time numbers are easy to manipulate. Trends are much harder.
>
> And I prioritize outcomes over activity. Features shipped per sprint is an activity metric. Customer adoption of those features is an outcome metric. Outcomes are what matter."

---

## QUICK REFERENCE TABLE — NUMBERS TO KNOW

| Metric | Your Target/Benchmark |
|--------|----------------------|
| Code coverage threshold | 90% minimum — enforced in CI |
| Sprint velocity improvement after AI tools | 15–20% over 15 months |
| P95 API latency target | 1 second (achieved in Optum migration) |
| P99 API latency | Monitored, spikes acceptable but alerted |
| Incident response: P1 MTTR | Under 30 minutes |
| Incident response: P1 detection (MTTD) | Under 5 minutes |
| Change failure rate | Below 5% of deployments |
| Delivery predictability target | Above 85% of committed features per quarter |
| Escaped defect rate | Below 5% |
| Tech debt allocation | 20% of every sprint |
| Platform health sprint | Once per quarter |
| OpenShift to Kubernetes saving | $9M annual ($24M → $16M) |
| Cerner incident rate reduction | 60% from legacy integration layer |
| Team size at Cerner | 14 engineers, 2 scrum teams |
| Clients served | 180 care management, 120 readmission prevention |

---

*File 3 of 6 — Delivery, Execution, KPIs & Agile*
