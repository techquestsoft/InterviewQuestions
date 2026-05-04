# Delivery & Agile Execution
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

---

## Q1: How do you manage end-to-end software delivery using Agile practices?

**Memory Trick:** Plan → Break → Execute → Inspect → Adapt

- **Plan with product** – Quarterly roadmap aligned with Product, broken into PI/release-level outcomes.
- **Break work into manageable units** – User stories with clear acceptance criteria, sized to complete within a sprint.
- **Execute with discipline** – Sprint planning, daily stand-ups, demos, and retros — not as ceremonies but as feedback loops.
- **Inspect with metrics** – Burn-up, predictability, defect rate, cycle time tracked per sprint and trended over quarters.
- **Adapt continuously** – Retros lead to concrete actions, not just discussion. Each sprint we improve one thing.

> Strong line: *"Agile is about predictable delivery and fast feedback, not about following ceremonies."*

---

## Q2: How do you ensure delivery predictability across sprints?

**Memory Trick:** Smaller Stories → Capacity Buffer → Track Trends → Address Root Cause

- **Smaller stories** – Stories sized to 2-3 days max. Large stories hide risk and slip silently.
- **Capacity buffer** – Plan ~70-80% of team capacity to absorb production support, unplanned work, and meetings.
- **Track planned vs actual** – Predictability ratio (committed/delivered) trended over 6+ sprints, not judged in a single sprint.
- **Address root cause of misses** – Was it estimation, dependencies, scope creep, or unclear requirements? Each has a different fix.
- **Visibility for stakeholders** – Predictability data shared openly so leadership trusts our commitments.

---

## Q3: How do you prioritize work when everything is urgent?

**Memory Trick:** Production → Compliance → Customer → Tech Debt

- **Production first** – Customer-impacting incidents and SLA breaches take priority. Always.
- **Regulatory/compliance next** – Hard deadlines with legal/audit consequences come before discretionary features.
- **Customer-facing features** – Prioritized by business value (revenue, adoption, customer journey impact).
- **Tech debt and platform work** – Allocated dedicated capacity (typically 15-20%) so it doesn't get crowded out.
- **Make trade-offs explicit** – When I say yes to A, I make clear what gets delayed. Priority is meaningful only when something is deprioritized.

---

## Q4: How do you handle scope changes mid-sprint or mid-release?

**Memory Trick:** Triage → Trade-off → Decide → Document

- **Triage urgency** – Is it truly P0/P1 or can it wait one sprint? Most "urgent" requests can wait.
- **Show the trade-off** – If it goes in, what comes out? Capacity is finite — adding work means cutting work.
- **Decide with stakeholders** – Product owns prioritization; I provide capacity reality. We decide together.
- **Document the change** – Capture the decision and its reason in JIRA/Confluence so the change is traceable.
- **Protect the team** – Never accept silent scope creep. Every addition is visible and accounted for.

---

## Q5: How do you do capacity planning and estimation?

**Memory Trick:** Historical → Capacity → T-Shirt → Refine

- **Use historical velocity** – Last 6 sprints' average is more reliable than any single sprint.
- **Account for true capacity** – Subtract leaves, on-call rotations, support load, meetings. Net capacity is usually 60-70% of headcount.
- **T-shirt size at PI level** – S/M/L/XL for epics during quarterly planning to set expectations.
- **Refine to story points at sprint level** – Detailed estimation only for the next 2 sprints. Don't waste effort on far-future estimates.
- **Re-estimate when scope clarifies** – Estimates are not commitments cast in stone; they evolve with understanding.

---

## Q6: How do you run effective sprint ceremonies?

**Memory Trick:** Purpose → Time-Box → Outcome

- **Sprint Planning** – Confirm sprint goal, break stories with team, commit only what fits capacity. Time-box: 2 hours per 2-week sprint.
- **Daily Stand-up** – 15 minutes max. Yesterday/today/blockers. Detailed discussions move offline.
- **Backlog Refinement** – Mid-sprint, ~1 hour. Refine next sprint's stories so planning is fast.
- **Sprint Review/Demo** – Show working software to stakeholders. Real demo, not slides.
- **Retro** – End every sprint with 1-2 concrete improvement actions, owned by named individuals, tracked next sprint.

> Strong line: *"Ceremonies that don't produce decisions or learnings are just meetings. I cut anything that doesn't earn its time."*

---

## Q7: How do you balance feature delivery with tech debt and platform work?

**Memory Trick:** Allocate → Bundle → Prove Value → Govern

- **Allocate dedicated capacity** – ~15-20% of every sprint for tech debt, platform work, observability improvements.
- **Bundle with feature work** – When touching a service for a feature, address adjacent debt. Reduces context-switching cost.
- **Prove value with data** – Show how a refactor reduced incidents 40%, or how an upgrade cut latency 30%. Builds case for future investment.
- **Maintain a tech debt backlog** – Visible, prioritized, refined just like feature backlog. Not a side note in someone's notebook.
- **Negotiate at the roadmap level** – I bring tech debt into PI planning explicitly so it's not a hidden trade-off.

---

## Q8: How do you use JIRA and other Agile tools effectively?

**Memory Trick:** Structure → Hygiene → Insight → Avoid Bureaucracy

- **Clear hierarchy** – Initiative → Epic → Story → Task. Each level serves a different audience (leadership/PM/team).
- **Backlog hygiene** – Stories have clear acceptance criteria, ownership, and status. Stale tickets cleaned weekly.
- **Use built-in reports** – Burn-up, sprint report, cycle time, control chart. Don't build custom dashboards if standard ones work.
- **Single source of truth** – No parallel spreadsheets. JIRA is the system of record.
- **Avoid bureaucracy** – I don't want engineers spending more time updating tickets than coding. The tool serves the work, not the other way around.

---

## Q9: How do you handle delivery delays or commitment misses?

**Memory Trick:** Surface Early → Root Cause → Recover → Prevent

- **Surface early** – The moment I see a risk, I flag it. Late surprises destroy trust; early flags build it.
- **Find root cause** – Was it estimation, scope, dependency, quality, or capacity? Generic "we'll do better" is not a fix.
- **Recovery plan** – Adjust scope, parallelize work, get focused help. Present the plan to stakeholders, not just the problem.
- **Communicate transparently** – Honest update with revised commitment. Don't keep "hoping" — hope is not a strategy.
- **Prevent recurrence** – Add the lesson to retro actions and to estimation/planning for next time.

---

## Q10: How do you measure delivery success beyond sprint velocity?

**Memory Trick:** DORA → Quality → Predictability → Outcomes

- **DORA metrics** – Deployment frequency, lead time for change, change failure rate, mean time to recover.
- **Quality** – Production defect rate, escaped defects per release, test coverage trend.
- **Predictability** – Commitment vs delivery ratio over time, not single-sprint velocity.
- **Customer/business outcomes** – Adoption, customer satisfaction, business KPIs the team's work supports.
- **Engineer experience** – Cycle time, PR review time, developer satisfaction. A team that delivers fast but burns out is not succeeding.

> Strong closing line: *"Velocity is an internal metric. I measure delivery success by what reaches the customer reliably."*

---
