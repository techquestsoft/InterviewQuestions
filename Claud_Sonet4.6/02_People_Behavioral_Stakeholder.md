# Interview Prep — File 2 of 7
# People Management, Behavioral & Stakeholder

> **Rule 1:** Management questions get management answers first. Technical design comes last, if at all.
> **Rule 2:** Every behavioral answer needs a specific example. Generic STAR scores 5/10. Specific STAR scores 9/10.
> **Rule 3:** Use STAR — Situation, Task, Action, Result — and keep the spoken version under 90 seconds.

---

## CROSS-FILE INDEX

This file owns: team management, behavioral STAR stories, leadership failure, stakeholder & conflict management.
- KPIs and delivery metrics → File 03
- Production incident management process → File 05

---

## SECTION A — TEAM MANAGEMENT

---

### Q1: How do you manage your team?

**Memory Hook:** Clarity → Track → Engage → Intervene

> "Four practices.
>
> First, clarity — every engineer knows exactly what they own and what success looks like. Specific, measurable commitments tied to the quarter's roadmap, not vague goals.
>
> Second, tracking — I monitor commitments versus delivery weekly in 1:1s and through sprint burndown. I do not wait for sprint review to discover slippage.
>
> Third, engagement — my 1:1s are not status updates. They are conversations about what is getting in the way, what the engineer wants to grow toward, and what I can remove.
>
> Fourth, intervention — when I see a blocker, I own removing it rather than waiting for the engineer to escalate.
>
> Real example: at Cerner, I inherited a team where engineers were context-switching across four parallel initiatives. Velocity was inconsistent, morale was low. I reduced active initiatives to two per engineer. Velocity improved 40% in six weeks with no personnel changes."

---

### Q2: How do you evaluate team performance?

**Memory Hook:** Delivery → Quality → Behavior

> "Three dimensions.
>
> Delivery — are they owning commitments reliably, flagging risks early, delivering what they said?
>
> Quality — what is their defect escape rate? How much rework do they produce? How is production stability in their areas?
>
> Behavior — are they collaborating, taking initiative, helping unblock peers, raising issues rather than hiding them?
>
> I differentiate expectations by level. Junior engineers are evaluated on execution and learning rate. Senior engineers on ownership and technical judgment. Architects on system-level decision quality and cross-team influence.
>
> I combine quantitative metrics with qualitative observations. Velocity alone is a proxy, not a verdict — a team completing 50 points of low-value work is less productive than one completing 30 points of high-impact work."

---

### Q3: How do you identify and grow high performers?

**Memory Hook:** Deliver → Own → Influence → Stretch → Visibility → Evidence

> "High performers reveal themselves through three signals: consistent delivery, proactive ownership beyond assigned scope, and influence over peers.
>
> When I identify them, I grow them through stretch assignments — not extra work, but different work. If someone wants to move into a tech lead role, I give them a feature to own end-to-end including stakeholder communication and cross-team coordination. I give the experience before the promotion, then make the case with evidence.
>
> For visibility, I rotate who presents in sprint reviews, who leads stakeholder discussions, who owns architecture deep-dives. High performers should be visible to the org, not just to me.
>
> For promotion, I build an evidence case — not narrative. 'Led Q3 incident response, mentored two engineers, delivered V3 integration on time' is evidence. 'Strong leadership potential' is not.
>
> Real outcome: 8 engineer promotions over 6 years at Optum, 2 promotions in 2 years at Cerner — all backed by documented evidence, not narrative."

---

### Q4: How do you handle low performers?

**Memory Hook:** Identify → Diagnose → System First → Improve → PIP if needed

> "My first question is always: is this individual or systemic? Unclear priorities, unmanaged interrupts, and missing psychological safety cause most low performance. I fix the system before coaching the individual.
>
> Real example: I had an engineer consistently missing sprint commitments. Before any feedback conversation, I pulled his velocity, code review turnaround, and QA feedback for the past quarter. Then I had a 1:1 — not a performance review, just an honest conversation about what was in the way.
>
> He was context-switching across production support, two features, and ad-hoc requests from another team. He had never raised this because it did not feel safe to.
>
> I restructured his allocation, set one focus area per sprint, and removed the cross-team requests formally. Six weeks later, velocity improved and QA friction dropped.
>
> If coaching does not move the needle after two sprint cycles, I escalate to a formal PIP with documented expectations, specific timelines, and clear consequences. That conversation is direct and honest — not adversarial, but not vague.
>
> Track record: 2 PIPs at Optum over 6 years — both resolved within plan, no surprise terminations."

---

### Q5: How do you build and motivate a high-performing team?

**Memory Hook:** Clarity → Ownership → Recognition → Growth → Connect to Impact

> "Four things.
>
> Clarity — people perform best when they know exactly what they are responsible for and why it matters. I connect sprint work to product outcomes explicitly, not just Jira tickets.
>
> Ownership — I assign responsibility, not just tasks. Ownership means an engineer cares about the outcome, not just completion. I create that by involving them in design decisions, not just handing them requirements.
>
> Recognition — I acknowledge strong contributions publicly in sprint reviews and team channels. Specific acknowledgment, not generic praise. 'How you handled the scope negotiation in Tuesday's planning showed mature judgment' lands. 'Good job' does not.
>
> Growth — I invest in IDPs (individual development plans) for every engineer. Where do they want to be in 12 months? What specific experiences do they need? I make those experiences available within our roadmap."

---

### Q6: How do you hire and build a team from scratch?

**Memory Hook:** Skill Matrix → Structured Assessment → Onboard for Retention

> "Three phases.
>
> Phase 1 — Skill matrix before requisitions. For a team supporting distributed systems and data pipelines, I need backend engineers strong in Java and event-driven systems, data engineers, QA automation, and someone with DevSecOps depth. I map the gap first, before opening any roles.
>
> Phase 2 — Structured assessment. ICs get coding and design rounds, senior engineers get system design, tech leads get leadership scenarios. Standardized rubrics for every role to reduce bias and make the process defensible.
>
> Phase 3 — Onboarding for retention. Most attrition happens in the first 90 days. I assign a buddy, set 30-60-90 day goals, and run weekly 1:1s from day one. Goal: engineer feels productive and connected within two weeks.
>
> I also serve as Bar Raiser at Cerner — across teams, not just my own — to help raise the overall hiring quality bar."

---

### Q7: How do you mentor and develop team members?

**Memory Hook:** Guide → Plan → Expose → Feedback

> "Mentoring is structured, not informal.
>
> I run quarterly development conversations with every engineer — separate from performance reviews. We discuss where they want to be in 12 to 18 months and what specific skills or experiences will get them there.
>
> Then I find those experiences within our roadmap. If someone wants to grow into architecture, I involve them in design reviews, not just implementation. If someone wants to present better, I put them in front of stakeholders with preparation support, not cold.
>
> Feedback is ongoing and specific. Not 'good job.' Specific: 'The way you handled the scope negotiation in last week's planning showed mature judgment — here is what I observed and why it matters.'"

---

### Q8: How do you scale an engineering organization?

**Memory Hook:** Structure → Platform → Standards → Grow

> "Four things matter when scaling.
>
> Structure — move from functional teams to domain-based teams with clear product ownership. Each team owns a domain end-to-end, not a layer of the stack.
>
> Platform — create a shared platform team that owns common capabilities: infrastructure, observability, CI/CD, GenAI tooling. Without this, every product team reinvents the same wheel.
>
> Standards — define what is non-negotiable across all teams: security gates, logging standards, incident response process. Teams decide their own tech stack within guardrails, sprint cadence, internal practices.
>
> Growth — balance hiring with internal upskilling. Hiring alone does not scale culture. I invest in coaching the people on the team before opening requisitions."

---

### Q8a: A developer says they're not getting challenging work — how do you handle it?

**Memory Hook:** Two Sprints Data → Normal 1:1 (NOT performance review) → Interest Area → System vs Capable → End-to-End Ownership Aligned to Roadmap

> "This is a growth conversation, not a performance conversation. The two are completely different and need different framing.
>
> First, I review two sprints of their data before the conversation — what they've actually delivered, what their work history shows in terms of technical depth, ownership, and problem-solving. I want to understand them before I sit down with them, not during.
>
> Then I have a normal one-on-one, explicitly not framed as performance management. The goal is understanding: what interests them, what would feel like growth, are there blockers I'm not seeing.
>
> If they show genuine capability AND clear interest in something specific — say, end-to-end ownership of a use case like our conversational AI POC — I align it with the quarterly roadmap. Not extra work piled on existing scope, but a meaningful piece of work that lets them visualize their contribution to the product itself, not just to code.
>
> That visibility — connecting their work to product outcome — is what turns a contributor into an engaged contributor.
>
> Real example at Cerner: I gave one engineer end-to-end ownership of our V3 Java/Spark POC. He owned the design, the implementation, and the architecture review with the HDI AI team. He went from feeling stuck on maintenance work to becoming the technical lead for that initiative."

**Important distinction from Q4 (low performer):**
- This question is about growth aspirations from a capable engineer
- Q4 is about an underperformer
- Same data-first principle, but completely different conversations

---

### Q9: How do you handle a team with low morale and missed deadlines (new manager scenario)?

**Memory Hook:** Listen 30 days → Diagnose 60 days → Quick Wins 90 days

> "The instinct when joining a struggling team is to act fast. That instinct is usually wrong. The right sequence is listen, diagnose, act.
>
> **Days 1–30 — Listen without agenda.** 1:1s with every team member. Skip-level conversations with key stakeholders to understand external perception. I am building a complete picture of pain points, not launching solutions.
>
> **Days 31–60 — Diagnose root causes.** Most common patterns I have seen: too many parallel priorities with no clear ownership; tech debt that makes every feature take 3x longer than estimated; no psychological safety so risks are not raised until they become incidents; the previous manager was a bottleneck rather than an enabler. I share my diagnosis transparently — 'Here is what I heard, here is what I think is happening, does this resonate?' This act of naming the problems openly starts rebuilding trust.
>
> **Days 61–90 — Quick wins.** I identify 2 or 3 things I can visibly fix quickly: a process bottleneck, a tool the team has been requesting, a blocking dependency. Quick wins build credibility. Simultaneously I set clear expectations going forward.
>
> Real example: at Cerner I took over a team where engineers were context-switching across four parallel projects. I reduced it to two per engineer. Sprint velocity improved 40% within six weeks — no personnel changes, no process overhaul."

---

## SECTION B — BEHAVIORAL (STAR STORIES)

---

### Q10: What are your key strengths?

**Memory Hook:** Ownership → Solve → Lead → Decide → Align

> "Five things I consistently bring.
>
> End-to-end ownership — I do not hand off problems. I see them from planning to production stability.
>
> Structured problem-solving — whether it is a production incident or a delivery conflict, I decompose before I act.
>
> Technical judgment — I make balanced decisions across scalability, reliability, cost, and timelines. I can speak both engineering depth and business impact.
>
> Business alignment — I connect engineering work to outcomes explicitly. Sprint metrics are means, not ends.
>
> Team leadership — I build teams that run independently, not teams that need me to function."

---

### Q11: What are your weaknesses?

**Memory Hook:** Depth → Structure → Delegate

> "Three areas I have actively worked on.
>
> Technical depth bias — earlier in my management career, I would answer management questions with technical solutions. I have learned to lead with the management answer and add technical context only when it genuinely adds value.
>
> Communication structure — I used to start with detailed context before the headline. I have shifted to summary-first communication, especially with senior stakeholders.
>
> Delegation — I used to retain critical problem-solving myself because it felt faster. I now recognize that as a scaling anti-pattern. I invest in the engineer doing the work, even if it takes longer initially."

---

### Q12: Tell me about a time you handled pressure well

**Memory Hook:** Blast Radius → Mitigate → Communicate Cadence → Learn

> "Our readmission prevention pipeline had a data processing failure affecting three health systems simultaneously. I owned the incident response.
>
> First, I assessed blast radius — three clients impacted, one approaching SLA breach. I declared SEV2 immediately, opened a war-room channel, assigned an incident commander.
>
> I communicated to stakeholders within 10 minutes: 'Three clients impacted. Root cause under investigation. Next update in 15 minutes.' I maintained that cadence throughout — every 15 minutes, even if the update was 'still investigating.'
>
> Mitigation first: we replayed Kafka from the last committed offset to restore data for the SLA-critical client. Restored in 47 minutes.
>
> Postmortem within 48 hours: surfaced a missing idempotency check that caused duplicate processing under retry. Fixed within the sprint. Zero recurrence for six months.
>
> What I carry from that: communicate early and often, mitigate before root-causing, and every incident should produce a structural fix — not just a workaround."

---

### Q13: Biggest Leadership Failure (Director-level Story)

**Memory Hook:** Plan for Ecosystem Readiness, Not Just Engineering Readiness

**60-second version (use this by default):**

> "I led the V1/V2 to V3 ML transformation at Oracle Cerner and committed PI timelines based on strong technical readiness — architecture finalized, platform aligned, data science model development planned.
>
> What I missed was validating a critical external dependency: client approval to use production data for model training. That approval required a 4–6 week legal and compliance review on the client's side. The PI commitment slipped by six weeks. A client stakeholder who had championed this internally had to reset expectations with their leadership — the trust cost was real.
>
> I took ownership immediately, communicated transparently with a recovery plan rather than just a delay notification, and restructured execution so the data science team worked with synthetic data during the approval wait. Zero idle engineering time.
>
> The structural fix was more important than the recovery: I introduced a dependency classification framework — Technical, Data, Legal, External Stakeholders — with a pre-commit gate. Before any PI timeline is signed off, all four categories are explicitly validated. In the very next initiative, that gate caught a data residency issue in week one instead of week eight. That project delivered on its original commitment.
>
> Core learning: in regulated ML systems, data readiness is a governed cross-functional dependency — not a technical assumption. I now plan for ecosystem readiness, not just engineering readiness."

**Closing line:**
> "This experience shifted my approach from planning for execution to planning for uncertainty."

---

### Q14: Tell me about an unpopular decision you made

**Memory Hook:** Security First → Data → Phased Plan → Own the Communication

> "We had a direct conflict between the compliance team requiring CrowdStrike security installation across all our Kubernetes and EMR instances — about a one-week effort affecting 120+ client environments — and the product team, who wanted us to continue the V3 ML initiative committed for the quarter.
>
> Both were legitimate. Security had an organizational mandate. Product had a client commitment.
>
> My position: security first. I made the case with data — in a healthcare system, a vulnerability exploit causes regulatory consequences and trust damage that far outweigh a one-sprint feature delay. I quantified both risks explicitly, not as opinion.
>
> I proposed a path forward that acknowledged both needs: complete CrowdStrike installation as priority, with a specific revised V3 delivery date so the product team could give clients an accurate timeline — which they preferred over uncertainty.
>
> I owned the stakeholder communication myself rather than leaving product to explain it alone.
>
> The product team agreed. CrowdStrike installed. V3 resumed one sprint later. No client escalation."

---

### Q15: Tell me about a time you disagreed with your manager

> "I share my perspective with data and context — once, clearly. If the decision goes the other way, I commit fully and execute. I do not relitigate settled decisions with the team.
>
> Real example: at Optum, I pushed back when leadership wanted to defer a tech debt initiative to make room for additional feature work. I presented the data — the specific debt was causing 40% of our production incidents from one integration layer. I proposed a single platform-health sprint per quarter as a compromise.
>
> Leadership initially declined. I committed to the original plan, executed the feature work, and kept tracking incident data. Two quarters later, the incident pattern made the case undeniable, and we got the platform-health sprint allocation. Within two quarters of that, the incident rate from that layer dropped 60%."

---

## SECTION C — STAKEHOLDER MANAGEMENT

---

### Q16: How do you manage conflicting priorities from multiple stakeholders?

**Memory Hook:** Shared Context → Transparent Capacity → Facilitate, Don't Referee

> "Conflicting priorities usually signal missing shared context — stakeholders are optimizing for their domain without seeing the full picture or engineering capacity.
>
> My approach: bring relevant stakeholders into one conversation with a priority matrix I have already drafted. I show engineering capacity transparently — here is what we can do this quarter, here are the options, you rank by business value.
>
> Engineering must never be the referee between business stakeholders. We facilitate, provide data, and surface trade-offs. The business owns the decision.
>
> For ongoing alignment, I run a monthly stakeholder sync — roadmap review, capacity update, upcoming trade-off decisions. Surprises are the enemy of trust. If a stakeholder first hears about a delay when a deadline is missed, I have failed at communication, not just at delivery."

---

### Q17: Business wants a feature in 6 weeks, team estimates 14 weeks. What do you do?

**Memory Hook:** Validate → Three Options → Ask Why → Never False Commit

> "This is a negotiation with data, not a standoff or a capitulation.
>
> First, I pressure-test the estimate. Are there conservative assumptions? Scope that can be phased? Dependencies that can run in parallel? Engineers sometimes pad estimates when they have not felt safe pushing back on scope before.
>
> Then I build a three-option proposal:
> - **Option A:** Full scope, done right, production-ready. 14 weeks.
> - **Option B:** 80% of business value, lower-priority features deferred to Phase 2. 6 weeks.
> - **Option C:** MVP deployed with real user feedback, full feature follows. 6 + 4 weeks.
>
> I present with business framing — not 'we cannot do it' but 'here are your options and the trade-offs of each.'
>
> Most importantly, I ask: what is driving the 6-week target? Is there a vendor contract deadline, a peak-season window, a board commitment? Understanding the real constraint often reveals flexibility I did not know existed.
>
> What I never do: commit to 6 weeks knowing it will take 14. That destroys trust and creates a death march for the team."

---

### Q18: How do you influence senior leadership decisions?

**Memory Hook:** Context → Options → Trade-offs → Data → Align

> "I focus on enabling decisions, not just presenting solutions.
>
> I start by understanding business context — what is leadership trying to achieve, what constraints are they operating under?
>
> Then I present options with explicit trade-offs — cost, risk, timeline. I never come with one recommendation hoping for agreement. Multiple options with clear implications signal I have thought through the problem.
>
> I support every option with data — system metrics, cost estimates, comparable precedents.
>
> And I stay open to the decision going differently. If leadership has context I do not, I want to hear it. My goal is a well-informed decision, not winning."

---

### Q19: How do you handle a difficult or resistant stakeholder?

> "I start by understanding what is driving the resistance. Most resistance is not irrational — it signals either the stakeholder has information I am missing, or they feel their concerns have not been heard.
>
> I schedule a direct conversation — not in a group setting. I ask what is most important to them and what they are worried about. I listen without defending.
>
> Then I look for overlap between what they need and what we can deliver. Often a phased approach or a small concession on timing resolves 80% of resistance.
>
> If resistance persists despite good-faith effort, I escalate — not as a complaint, but as a decision needing leadership input. I bring both perspectives, the trade-offs, and a recommendation."

---

### Q20: How do you build trust with stakeholders?

**Memory Hook:** Be Clear → Be Consistent → Be Collaborative

> "Three practices.
>
> Transparency — I share honest updates, including bad news early. A stakeholder who learns of a risk from me before it becomes an incident trusts me. One who learns of it after it blows up does not.
>
> Reliability — I deliver on commitments. When I cannot, I communicate early with a revised plan. Never silence.
>
> Collaboration — I involve stakeholders in decisions rather than presenting outcomes. People support what they help build."

---

### Q21: External API is unreliable — what do you do?

**Memory Hook:** Protect → Monitor → Govern → Fallback

> "Four-layer response.
>
> **Protect** — implement retries with exponential backoff, timeouts, and circuit breakers using Resilience4j. The external system's instability cannot become my system's instability.
>
> **Monitor** — track latency and failure patterns by dependency. Separate dashboards for external versus internal failures so we can distinguish 'their problem' from 'our problem' in seconds, not hours.
>
> **Govern** — define SLAs and escalation paths with the vendor. If they breach the SLA, we have a documented path with their support team.
>
> **Fallback** — design fallback strategies. Cached responses for read paths. Queued requests for write paths. Graceful degradation on the UI rather than full outage.
>
> Real example: our authorization dependency on Faraday had transient network instability causing intermittent 500s. We added retries with exponential backoff, timeouts, fallback handling, and dependency-level monitoring. Customer impact was significantly reduced."

---

## QUICK REFERENCE TABLE — MEMORY HOOKS

| Topic | Memory Hook | One-Line Answer |
|-------|------------|----------------|
| Team management | Clarity → Track → Engage → Intervene | Clear goals, weekly tracking, real 1:1s, remove blockers fast |
| Low performers | Identify → Diagnose → System First → PIP if needed | Fix system before individual, PIP only if coaching fails |
| High performers | Deliver → Own → Influence → Stretch | Stretch work, visibility, evidence-based promotion |
| Hiring | Skill Matrix → Structured Rubric → 90-day onboarding | Gap first, calibrated rubric, retention plan |
| Stakeholder conflict | Shared Context → Transparent Capacity → Facilitate | One conversation, show capacity, business decides |
| 6 vs 14 weeks | Validate → Three Options → Ask Why → Never False Commit | A/B/C framework, ask what drives the date |
| Leadership failure | Ecosystem Readiness, not just Engineering | Data governance is critical path, not assumption |
| Unpopular decision | Data → Phase → Own the Communication | Security over feature, phased plan, own the messaging |
| Influencing leadership | Context → Options → Trade-offs → Data → Align | Enable decisions, not just present solutions |
| External API failure | Protect → Monitor → Govern → Fallback | Resilience4j patterns + vendor SLA + degradation |

---

*File 2 of 7 — People Management, Behavioral & Stakeholder*
