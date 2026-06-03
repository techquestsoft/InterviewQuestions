# Optum OCM — Round 3 Final Prep (Wednesday 12pm)
**Contact: Srikanth (Senior Director) — same as Round 2**
**Format: In-person, 60–90 minutes. Possible additional interviewers: Peer Director or Principal Engineer**

---

> **The one thing to remember walking in:** This round is yours to lose, not win from scratch.
> Two rounds done. He invested heavily selling you the role in Round 2. He's assessing presence, trust, and fit — not re-screening your resume.

---

## 1. Your Opening — Two Versions

> **Key rule:** He already heard the Cerner/Kubernetes story. Lead with **management shape** and end with **why this role specifically**. Don't replay Round 2.

### 30-second version
> "I lead engineering teams across healthcare and banking platforms — most recently at Oracle Cerner, where I ran Care Coordination products serving 500 million patient records across 120-plus customers. Before that I delivered a $5M Kubernetes modernization at Optum. I'm drawn to this role specifically because prior-auth is at exactly the intersection of platform scale and AI-driven clinical decision support — which is where I've been focusing most of my energy these last two years."

### 60-second version (standard)
> "I'm a Senior Engineering Manager with 20-plus years across healthcare, banking, and insurance. At Oracle Cerner I led two Care Coordination products — a risk stratification engine and a care gap analytics suite — serving 120-plus customers and 500 million patient records. I ran 14 engineers across two scrum teams, accountable end-to-end from backlog to production. Before that, at Optum under the EIP program, I led a $5M cloud-native Kubernetes migration, ramped four teams, and built engineering practices around GitHub Actions and automated pipelines.
>
> What's brought me to this conversation is a deliberate choice. I want to work on a platform where AI isn't a side project but a funded program with clinical stakes. The prior-auth problem, the 70-30 traditional-to-AI split you described, the behavioral and post-acute expansion — that combination of scale, domain complexity, and AI maturity is why I'm here."

---

## 2. Questions Srikanth Is Likely to Ask

---

### Q1: "What would you do differently in your first 90 days here vs what you've done before?"

**Memory Hook:** Listen → Quick Wins → One Structural Point of View

> "The first 30 days I'd listen more than I act. 1:1s with every engineer and tech lead — not to assess them, but to understand what's working, where they feel blocked, and what they're proud of. I'd shadow sprint ceremonies without changing them immediately. And I'd read the last three post-mortems and the current tech-debt backlog — those two things tell me more about a team's real state than any architecture diagram.
>
> Days 30–60, I'd build trust by removing one or two real blockers — things annoying the team that haven't been fixed. Quick wins that show I'm there to serve the team, not audit it.
>
> By day 90, I'd have a clear point of view on the one structural thing I'd change — process, dependency, tooling gap, or ownership boundary — and I'd bring that to you with data, not opinion. The mistake I've made before is forming that opinion in week two and acting on it in week three. This time I'd earn the credibility first."

---

### Q2: "Tell me about a time you had to make a decision your team disagreed with."

**Memory Hook:** Own the call → Explain once → Don't re-argue → Let evidence build

> "At Oracle Cerner, when planning the cloud migration from AWS to OCI, the team strongly preferred a lift-and-shift approach — get off AWS fast, optimize later. I disagreed. My assessment was that a lift-and-shift on our OpenSearch indexes would create a fragile, expensive-to-operate state we'd be stuck with for two-plus years.
>
> I made the call to do a parallel-run migration with weekly validation gates — keep AWS alive, run both environments, cut over customer by customer, shut down AWS only after 30 days of clean validation. The team pushed back hard — more work, more coordination, longer timeline.
>
> I explained my reasoning once clearly and didn't revisit the debate. I gave each lead ownership of their migration stream so they had real accountability, not just tasks. By the end, three of the five leads told me they'd changed their minds midway when they saw the first two lift-and-shift attempts catch real issues in the parallel environment.
>
> Outcome: zero customer-impacting incidents during migration. If we'd lifted and shifted, I'm confident we'd have had at least two major outages."

---

### Q3: "How do you manage engineering quality under delivery pressure?"

**Memory Hook:** 20% buffer → CVSS 7+ hard line → PI planning for big debt → Impact data for product

> "I run a 20% sprint buffer protected for tech debt and quality work — not negotiable with product unless we're in a genuine P0 situation. I don't frame it as 'taking time away from features.' I frame it as 'this is what keeps our velocity predictable.' A team that skips quality work for three sprints doesn't go 20% faster — they go 20% faster for six weeks and then grind to a halt.
>
> For security specifically, I hold a hard line on CVSS 7-plus vulnerabilities — high-severity security issues that must be fixed immediately. Those go to the top of the backlog regardless of sprint plan, because the cost of a breach in a healthcare platform is not a trade-off I can make for delivery speed.
>
> For larger work — Java version upgrades, Spring Boot, library consolidation — I scope and estimate them separately and bring them to PI planning as their own line items with business impact data: latency numbers, incident history, what it costs us to keep carrying the debt. When product sees 'this is causing three incidents per quarter and will cause a major outage within six months,' they stop treating it as optional.
>
> I've never had a product manager fight me on tech debt once I've shown them the cost of not doing it."

---

### Q4: "How do you think about building an AI team within an existing engineering org?"

**Memory Hook:** Two tracks → Productivity AI (embed) vs Clinical AI (pod + governance) → Human-in-the-loop always

> "I think about it in two tracks.
>
> The first is productivity AI — GitHub Copilot, code assist, prompt-structured test generation. This doesn't need a separate team; it's an engineering practice embedded across all your existing squads. The governance work here is around prompt standardization, manual PR review staying non-negotiable, and measuring the right things — not 'are people using it' but 'is code review time down, is test coverage up, are escaped defects trending down.' At Cerner we saw about 20% productivity improvement over 15 months.
>
> The second track is clinical AI — where the model output has patient impact. This needs different governance. I'd structure it as a pod embedded within the product squad: an ML engineer, a data engineer, a domain expert, and a dedicated QA person focused on output validation. The key principle I hold is human-in-the-loop for any recommendation that a care manager or clinician acts on. The model surfaces and ranks; a human decides. In a CMS-regulated prior-auth environment, that's not just a safety choice — it's the only defensible architecture.
>
> The hardest part isn't the model — it's grounding. If you don't have clean, consistent source-of-truth data that the model can reference, you get confident-sounding wrong answers, and in healthcare that's a clinical risk, not just a UX problem.
>
> One concrete example from Cerner: for our readmission risk scoring POC, we evaluated both Claude and ChatGPT as the underlying model. We ran accuracy comparisons against our known risk scores and Claude performed better for our use case. A key infrastructure advantage was OCI's LLM setup — all inference traffic stays inside OCI's infrastructure, it never goes to the public internet. In a HIPAA environment, that containment is not optional."

---

### Q4a: "Walk me through your AI guardrails and governance — how are they different for productivity AI vs clinical AI?"

**Memory Hook:** Who bears the harm → Different risk = different framework → Productivity (code quality) vs Clinical (patient safety)

> "The starting point is who bears the harm if the AI gets it wrong. For productivity AI, a bad output means a bad PR — the harm is a code quality issue, caught in review. For clinical AI, a bad output means a wrong recommendation acted on by a clinician — the harm can reach a patient. That difference drives completely separate frameworks."

#### Productivity AI Guardrails

**Input rules:**
> "No PHI in prompts — ever. Engineers under deadline pressure will paste real patient records to generate test data. That data travels to an external model endpoint and may leave your HIPAA boundary. The rule is absolute: use synthetic data or fixture files, never real records. Beyond that, structured prompt templates — the `skills.md`, `review.md`, `test-generation.md` approach — so every developer uses the same format and output is consistent and reviewable."

**Output rules:**
> "AI-generated code is a first draft, same status as code from your most junior engineer. No auto-merge, no auto-commit. Every AI-generated line goes through a human PR review. The engineer who accepted the suggestion is accountable for it, not the model. And test coverage is non-negotiable — if AI generated the implementation, the tests must still be written and critically reviewed by a human. An AI grading its own homework is not governance."

**What I measure:**
> "Not 'are people using it' — that's a vanity metric. I measure outcomes: code review cycle time trending down, test coverage trending up, escaped defect rate to UAT and production trending down. At Cerner we saw about 20% productivity improvement over 15 months. If the tools aren't moving those numbers, the governance overhead isn't justified."

#### Clinical AI Guardrails

**The non-negotiable: human-in-the-loop**
> "No clinical AI recommendation goes directly to a care action without a human reviewing it. The model surfaces, ranks, and explains. A human decides. This is not a design preference — it is the only defensible architecture in a CMS-regulated environment, and the only honest answer to a compliance auditor."

**Grounding — what goes into the model:**
> "The model must be anchored to specific, authorized sources of clinical truth — your patient records, your clinical criteria database, your authorization history. RAG — Retrieval-Augmented Generation — is the mechanism: before the model generates a response, it retrieves relevant documents from your verified store and generates only from those. Without grounding, the model reasons from its training data, which may be outdated or hallucinated. On Azure, this maps to Azure OpenAI Service with private endpoints and data residency controls — not the public OpenAI API. That containment is a hard HIPAA requirement."

**Output validation before anything reaches a user:**
> "Three things must be true before a clinical AI output surfaces to a clinician. One — the recommendation cites a specific, retrievable source. If it can't, the claim is suppressed. Two — confidence is above a defined threshold; below threshold, the output is flagged 'low confidence — manual review required.' Three — explainability: every recommendation shows which data points drove it. 'High readmission risk' is not acceptable output. 'High readmission risk — based on: prior 30-day admission, CHF diagnosis, three missed medication refills' is."

**Staged rollout — never POC straight to production:**
> "Shadow mode first — the model runs in parallel with the existing workflow, its outputs are logged and reviewed internally, but nothing it says affects any care decision. You run shadow mode until you have hundreds or thousands of cases to validate accuracy against ground truth. Only then does output become visible to users, and initially only to a defined pilot group. The moment accuracy degrades — caught by weekly manual QA sampling or automated output distribution monitoring — the feature is disabled pending investigation."

**Audit trail:**
> "Every model inference that influences a clinical decision must be logged: input, retrieved context, output, what the user did with it, timestamp, user ID. Immutable, retained per HIPAA data retention requirements. This is how you demonstrate human oversight was maintained at every step if you're ever audited."

**The insight that separates lived experience from theory:**
> "The hardest governance problem isn't the model — it's the data. If your clinical data is inconsistent, incomplete, or incorrectly labeled, a well-governed model will still produce wrong recommendations. Data quality governance has to come before AI governance, not after."

#### Spoken summary (use this if they ask for the short version)
> "Two completely separate frameworks because the risk profiles are different. For productivity AI — no PHI in prompts, structured templates, PR review stays human and mandatory, measure outcomes not usage. For clinical AI — human-in-the-loop always, grounding to verified sources only via RAG, output validation before anything reaches a clinician, shadow mode before any user-visible rollout, and an immutable audit trail for every inference. The thing I've learned from actually building this: data quality governance has to come before AI governance. A well-governed model on dirty data still produces wrong answers."

---

### Q4b: "How did you evaluate and compare LLM models? Walk me through your methodology."

**Context:** Srikanth asked this directly in Round 2 — *"Did you try multiple models to see which model gave you a better response? How did you go about prompt engineering?"* Your live answer told him the result (Claude won) but not the evaluation process. A Principal Engineer will probe this deeper.

**Memory Hook:** Define the task → Build the test set → Same prompt both models → Score on 4 dimensions → Infrastructure check → Document and decide

> "We approached it like any engineering evaluation — define what you're measuring first, then run both models against the same inputs and score against known ground truth.
>
> The task was specific: given a natural language query from a care manager — say, 'show me the top 10 patients with the highest readmission risk this week' — the model needed to correctly identify which tool to invoke, pass the right parameters to OpenSearch, retrieve the right patient records, and format the response clearly for a non-technical care manager. That gave us four measurable dimensions: tool selection accuracy, parameter correctness, result accuracy against our known risk scores, and response clarity.
>
> We built a test set of around 30 representative care manager queries — simple lookups, ranked lists, filtered queries, and edge cases like ambiguous date ranges or missing parameters. We ran both Claude and ChatGPT against the same test set with the same system prompt and the same tool definitions. We scored each response manually against ground truth — the actual OpenSearch query we'd have written by hand for that intent.
>
> Claude outperformed on two dimensions specifically: tool parameter accuracy — it was less likely to hallucinate parameter values when the query was ambiguous — and it handled the care manager persona more naturally in its response phrasing. ChatGPT was slightly more verbose and occasionally generated plausible-sounding but incorrect filter values.
>
> Beyond accuracy, there was an infrastructure dimension. OCI had both models available within their LLM infrastructure — all inference stays inside OCI, never reaches the public internet. In a HIPAA environment that containment is non-negotiable. On Azure, the equivalent is Azure OpenAI Service with private endpoints — not the public OpenAI API.
>
> On prompt engineering: we used system prompting to define the care manager persona — who they are, what data they care about, how readmission risk scores are used in their workflow, what response format they expect. That system context is what stopped the model from giving a generic data analyst response and kept it grounded in the clinical workflow. We also defined the tools explicitly — OpenSearch tool and RAG tool — with clear descriptions of when each should be invoked, so tool selection was guided, not guessed."

**If they push — "how would you scale this for a production model selection?"**
> "For production I'd formalize the test set into a regression suite — golden queries with expected outputs that runs every time we consider switching or upgrading a model. I'd add automated scoring where possible: exact-match for tool selection and parameter values, semantic similarity scoring for response quality using embedding comparison against reference answers. Manual review stays in the loop for clinical accuracy — automated scoring can tell you a response is structurally correct but only a domain expert can tell you it's clinically correct. I'd also add latency and cost to the evaluation matrix — a model that's 5% more accurate but 3x slower or 10x more expensive may not be the right production choice at 70-80k authorizations per day."

---

### Q5: "What's your leadership style and how does it adapt?"

**Memory Hook:** Coaching by default → Adapts by experience + urgency → Directive in crisis

> "I default to coaching over directing — I'd rather ask the right question than give the answer, because the person closest to the problem usually has a better answer than I do. But I adapt based on two variables: the engineer's experience level and the situation's urgency.
>
> With a senior engineer or tech lead, I set the outcome and stay out of the how. With someone newer, I stay closer — pair on design, review more frequently, give explicit feedback on the work, not just the result.
>
> In a major outage or high-stakes delivery crunch, I shift gears — more directive, faster calls, shorter debate. Teams appreciate that clarity in a crisis even if they wouldn't want it every day.
>
> The failure mode I watch for in myself is over-coaching when a situation just needs a decision. I've had to learn to recognize when someone doesn't want a guided thinking session — they want me to say 'do it this way' and move on."

---

### Q6: "Srikanth told you the success metrics in Round 2 — connect your answer to them"

**Use this when he asks "how will you measure your own success" or "what does good look like in 6 months"**

> ⚠️ **This is critical** — Srikanth already told you exactly how he measures success in Round 2. Echo his own words back with your plan against each metric.

He said:
- **Quality** — defect rates across unit testing, E2E testing, and UAT before monthly releases
- **Engineering productivity** — they have explicit metrics and guidelines; teams must stay within them
- **Stakeholder alignment** — working with TPOs and architects to maintain a healthy backlog for 3-sprint PI planning (2-week sprints)
- **Growth path** — Grade 29 → Director, expanding from inpatient to behavioral and post-acute lines

**Your answer:**
> "You actually gave me a clear picture in our last conversation. Quality — reducing defect escape rate across unit, E2E, and UAT stages before your monthly releases. Engineering productivity — staying within the org's defined metrics, and honestly pushing them higher using AI tooling the way I've done before. And backlog health — making sure the teams are never starved for well-groomed work going into each PI cycle.
>
> In the first 90 days, I'd establish a baseline on all three before I try to move any of them. You can't improve what you haven't measured first. By month six, I'd want to show a visible trend — fewer defects escaping to UAT, productivity metrics moving in the right direction, and a backlog that's consistently two sprints deep and well-estimated."

---

## 3. If a Peer Director Joins

### Q7: "How do you handle a situation where another team's dependency is blocking your sprint?"

> "First I try to resolve it at the team level — direct conversation between my tech lead and their counterpart. Most dependency issues are coordination problems, not conflict, and they resolve there without escalation.
>
> If it doesn't resolve in 48 hours, I pick up the phone with the other EM directly — peer-to-peer, no hierarchy: 'here's the impact on my sprint, here's what I need, what do you need from me to unblock this?' That resolves it 80% of the time.
>
> If there's a genuine resource or priority conflict neither of us can resolve, I escalate with data — not as a complaint but as a trade-off for leadership to make. I don't ask leadership to solve it; I ask them to make the priority call and I execute it.
>
> What I never do is let a dependency sit quietly and slip the sprint without flagging it early. The damage from a late flag is always worse than the awkwardness of an early one."

---

### Q8: "How do you share engineering practices across teams?"

> "I run a fortnightly engineering sync — not a status meeting, a practice-sharing session. One team brings something each time: a post-mortem, a design decision they made, a tool they adopted. 30 minutes, attendance expected, builds a shared vocabulary across teams.
>
> For standards that need to be consistent — security practices, API design, PR review norms, AI tooling governance — I document them in a lightweight decision record and make them part of onboarding. Not a wiki page no one reads — a file that's referenced in the repo and reviewed in code review.
>
> Engineers resist top-down standards but adopt peer-driven ones. So I make sure the person presenting the practice is the engineer who built it, not me. I facilitate; they own."

---

## 4. If a Principal Engineer Joins

### Q9: "How do you make architectural decisions? Walk me through your process."

**Memory Hook:** Two-way vs one-way door → Tech lead brings options → Three questions always → Document in ADR

> "I start by separating the decision from the conversation. Is this a two-way door or a one-way door? Reversible decisions I push down to the tech lead. Irreversible or expensive-to-change decisions I get involved in directly.
>
> For a real architecture decision, I ask the tech lead to bring three things: the options they considered, the trade-offs they see, and their recommendation with a reason. I'm there to stress-test the reasoning and make sure they've thought about operational cost, not just build cost.
>
> Three questions I specifically ask that engineers often skip: what does failure look like, how do we observe this in production, and what's our backup plan if we get this wrong? If those don't have good answers, the decision isn't ready.
>
> I document the decision in an ADR — Architecture Decision Record — context, options considered, the decision, and consequences. Six months later, when someone asks 'why did we do it this way,' the answer exists."

---

### Q10: "How do you handle technical debt in a fast-moving product team?"

> "I treat tech debt like a backlog item with a business cost, not a technical complaint. If I can't quantify the cost — slower delivery, higher incident rate, harder onboarding, security exposure — I can't prioritize it against feature work.
>
> My operating model: 20% buffer every sprint. Hard mandatory fix for CVSS 7+ vulnerabilities. For bigger structural work — runtime upgrades, library consolidation — scope it separately and present at PI planning with a cost-of-delay argument.
>
> A team that never pays down debt doesn't slow gradually — it falls off a cliff. I've seen it: a team that deferred Spring Boot upgrades for 18 months and then spent a full quarter firefighting when dependencies reached end of life. That's the story I tell product when they push back."

---

### Q11: "How do you manage API version proliferation?"

> "We had 16 active API versions in the orders domain — built up over 15 years of additive change. The cost was real — 16 versions of infra, 16 sets of tests, consumers spread across all of them.
>
> My approach going forward: cap active versions at two or three maximum. Additive changes are version-transparent — new fields ignored by consumers who don't use them, so no new version needed. Breaking changes get a new version with a six-month deprecation window and tooling to help consumers migrate. Actively track which consumers are on which version and decommission once traffic drops to zero.
>
> The lesson: API versions are like branches in source control. A few are fine. Sixteen means you stopped merging a long time ago."

---

### Q12: Production debugging — exponential backoff / retry pattern

> "In our Faraday authorization service we were seeing 500 error spikes — two to three seconds of failures every hour. Caught during daily dashboard review, not customer-impacting yet.
>
> Root cause: transient network failures between our service and the Faraday platform. Instead of just accepting that dependency, we introduced retry with exponential backoff: first retry after 1 second, second after 2 seconds, third after 4 seconds. Only after all three retries fail does the request count as a real failure.
>
> The principle: don't let a transient infrastructure issue become your application's fault. Retry with backoff, limit to three attempts to avoid amplifying load on a struggling downstream, escalate to a real error only after you've exhausted the window."

---

### Q13: Java 11 vs Java 17 vs Java 21

> "We were on Java 11 and planned a phased move to Java 17. Java 11 reached end of free Oracle support — a security risk since vulnerabilities stop getting patched.
>
> Main migration work: scanning libraries that used internal JDK APIs now restricted in Java 17, using the `jdeps` tool before touching any code. We also refactored key DTOs into Records — a Java 17 feature that removes repetitive boilerplate. Planned as a separate sprint item, not mixed into feature work — mixing migrations with features makes rollback very hard.
>
> Java 21 is on the radar specifically for Virtual Threads — makes Java handle thousands of concurrent tasks much more efficiently without rewriting code. For a system like prior-auth handling 70-80k authorizations per day with lots of I/O, Virtual Threads could meaningfully improve throughput. Not committed to that timeline yet."

---

### Q14: React / Frontend — your honest story

> "My frontend exposure has been at the integration level rather than deep development. In our Care Management and Readmission Prevention products, we had React-based UI components built as pluggable modules inside the Cerner MPages clinical workstation framework. My involvement was making sure those components integrated cleanly with our backend APIs and that data contracts were right. Direct React enhancements were limited, so I won't claim deep React expertise. What I do bring is a clear understanding of the frontend-backend contract, API design for UI consumption, and how to work effectively with frontend engineers to define what they need from the backend."

---

### Q15: React — high-level engineering manager questions

**"How do you ensure React frontend quality without being a React developer yourself?"**
> "I hold the quality bar at the contract boundary — the API contract between frontend and backend — and make sure frontend engineers own everything inside their component boundary. Concretely: well-defined versioned API contracts, component-level unit tests using React Testing Library, and end-to-end tests covering critical user flows — in a prior-auth context that's the full authorization submission and status tracking flow. PR reviews for React code go through a frontend-experienced engineer. I can read a component and understand whether it's managing state cleanly or leaking side effects — enough to ask the right questions."

**"What is your view on state management in a large React application?"**
> "The biggest risk is state sprawl — state that starts local and gets prop-drilled five levels deep, or duplicated across components. For a focused prior-auth submission form, useState and useContext is usually enough. When the application grows to multiple workflows sharing clinical data — authorization status, member details, clinical criteria — you need centralized state. Redux is established for complex state with audit trails. Zustand and React Query are lighter and more modern for server-state synchronization, which is mostly what a prior-auth UI does. I wouldn't prescribe a tool — I'd ask the frontend tech lead to justify their choice against the actual product complexity."

**"Prior-auth has complex multi-step workflows. How do you approach that?"**
> "Multi-step clinical workflows — member lookup, clinical criteria entry, documentation upload, submission confirmation — are exactly where state management and form validation become critical. I'd ensure the team designs the workflow as a state machine: each step has defined states, valid transitions, and a clear definition of 'complete' before proceeding. I'd also push for auto-save drafts so a care coordinator who gets interrupted doesn't lose work. And backend APIs must support partial saves, not just all-or-nothing submission, so frontend and backend failure modes align."

**"How do you handle accessibility in a clinical application built in React?"**
> "Accessibility is a compliance and usability requirement in a clinical application — not optional. I'd make it part of the definition of done for every UI story: semantic HTML first, ARIA labels for complex clinical data tables and dynamic status indicators, keyboard navigation covering the full authorization workflow, and axe-core in the CI pipeline so issues are caught before code review, not after."

---

### Q16: Kafka Idempotency

**Memory Hook:** Two separate problems → Producer retries (enable.idempotence) vs Consumer reprocessing (your app's job) → Dedup key must be deterministic

> "There are two separate things — interviewers often treat them as one.
>
> `enable.idempotence` is producer-side: Kafka assigns a Producer ID and stamps every batch with a monotonically increasing sequence number per partition. The broker tracks the highest sequence number written for each producer-partition pair. When a retry arrives with the same sequence number, the broker discards it silently. Protects against producer retry duplicates — within a single session, within a single partition.
>
> What it does NOT cover is consumer-side reprocessing. Kafka's consumer guarantee is at-least-once — crash after processing but before committing offset, and on restart you reprocess. That's legitimate redelivery; producer sequence numbers never come into play.
>
> Consumer-side idempotency is entirely my application's responsibility. The dedup key must be derived deterministically from the message content — not generated fresh at processing time. A business key like `patientId + clinicalEventId + sourceVersion`, or an idempotency key assigned once at the origin and carried with the message. Then enforce at the consumer with a dedup store — Redis or DB uniqueness constraint — or an idempotent upsert keyed on the business key."

---

### Q17: Rate Limiter

**Memory Hook:** Confirm constraints → Static variable fails across pods → Redis atomic counter → 429

> "First confirm the constraint: enforced in-process, no external gateway, per-consumer limit. The catch is multiple pods in Kubernetes — I cannot use a static variable, each pod only sees its own traffic slice and can never enforce a global limit.
>
> Put the counter in Redis and enforce it in a Spring interceptor before the controller. Read the consumer ID from the request header, atomic increment against a Redis key scoped to that consumer and time window. Within limit — proceed. Exceeds limit — return 429 with Retry-After. Redis gives the atomic operation and TTL resets the window automatically.
>
> For algorithm: fixed window is simplest and fine for most cases. Token bucket gives smoother shaping if boundary bursts are actually causing problems. Start simple, upgrade only when needed."

---

## 5. Your Questions for Srikanth

> **Rule:** Ask at least 2. Ask things only *he* can answer from running the platform.

- **On AI programs:** "You mentioned AI has dedicated capital this year. How are you thinking about the boundary between productivity AI for the engineering team and clinical AI in the product — are those governed the same way, or fundamentally different programs?"
- **On expansion roadmap:** "With behavioral health and post-acute care onboarding — what's the biggest engineering challenge in supporting multiple clinical lines of business on the same prior-auth core? Is it the data model, the tenancy architecture, or something else?"
- **On what he needs:** "What's the one thing the team needs most from this hire that isn't obvious from the job description?"
- **On success:** "What does success look like for you in this role after 12 months — how will you know this was the right hire?"
- **On accountability model:** "You described the org as flat and accountable — teams own outcomes end-to-end. How does that work in practice when a team hits a genuine dependency on another team's roadmap?"

---

## 6. Key Terms Simplified

| Word | Simple version |
|---|---|
| **Relitigate** | Re-argue, debate again |
| **Brittle** | Fragile — easy to break, not flexible |
| **Grind** | Push through, work hard on it |
| **Socratic** | Guiding by asking questions, not giving answers |
| **ADR** | Architecture Decision Record — short document recording why a decision was made |
| **EOLed** | End of Life — vendor stopped supporting it, no more security patches |
| **CVSS 7+** | Security vulnerability score 7 or above out of 10 — high severity, must fix immediately |
| **Mitigation path** | Backup plan if something goes wrong |
| **RAG** | Retrieval-Augmented Generation — model retrieves verified documents first, then generates answer only from those |
| **Shadow mode** | AI runs in parallel, outputs logged internally, nothing affects actual decisions — used for accuracy validation before go-live |
| **Model drift** | When a model's accuracy degrades over time because real-world data shifted from training data |
| **Golden test set** | A fixed set of representative queries with known correct answers — used to evaluate and compare LLM models objectively |
| **enable.idempotence** | Kafka producer setting — prevents duplicate writes from producer retries using Producer ID + sequence numbers |
| **At-least-once delivery** | Kafka consumer guarantee — a message may be delivered more than once if consumer crashes before committing offset |
| **Idempotent upsert** | A write that is safe to repeat — same input always produces same result, no duplicates |
| **Token bucket** | Rate limiting algorithm — allows short bursts but holds the average rate steady |
| **Exponential backoff** | Retry strategy — wait 1s, then 2s, then 4s so you don't hammer a struggling service |
| **State machine** | A design pattern where a workflow has defined states and controlled transitions |
| **axe-core** | Open-source accessibility testing library — runs in CI to catch accessibility issues automatically |
| **DBA (wrong word)** | Don't use this. Say "a care manager with a panel of patients" |
| **Prior Authorization** | Insurance company approval required before a treatment or test proceeds |
| **Grade 29** | Optum's internal level for Senior Engineering Manager — next level is Director (Grade 30), spanning 6–8 teams |

---

## 7. Things NOT to Do Wednesday

- **Don't replay Round 2 stories verbatim.** Build on them, don't replay them.
- **Don't say "DBA"** — say "a care manager with a panel of patients."
- **Don't give framework answers** — he wants a specific story: situation, your decision, outcome.
- **Don't say "No" when he opens the floor for questions early** — engage early, you missed this in Round 2.
- **Don't undersell management scope** — lead with "14 engineers across two scrum teams" before "$5M migration."
- **Don't use a static variable for rate limiting** — Redis atomic counter, static state doesn't survive across pods.
- **Don't give a generic success answer** — he told you his metrics in Round 2. Echo them back.
- **Don't conflate productivity AI and clinical AI governance** — separate frameworks, different risk profiles.
- **Don't just state the LLM evaluation result** — if model comparison comes up, explain the methodology: test set, four dimensions, same prompt both models, then the result.

---

## 8. Your Strongest Cards — When to Play Them

| Card | When to play it |
|---|---|
| **$5M Kubernetes migration at Optum** | Delivery credibility, cloud-native experience |
| **120+ customers, 500M records at Cerner** | Scale and accountability scope |
| **Conversational UI / RAG POC with human-in-the-loop** | AI governance, clinical AI — most differentiated story |
| **Claude vs ChatGPT evaluation — 4-dimension methodology** | When LLM model selection or evaluation comes up |
| **OCI / Azure OpenAI private endpoints for HIPAA** | When infrastructure and AI security comes up |
| **Two-track AI governance framework** | When AI guardrails or governance comes up |
| **Data quality before AI governance** | The insight that separates lived experience from theory |
| **20% sprint buffer + CVSS 7+ mandate** | Quality vs delivery pressure |
| **Parallel-run migration with weekly validation gates** | Risk management, decision-making under pressure |
| **Four-team ramp at Optum EIP** | Team-building, scaling engineering capacity |
| **~20% productivity improvement from Oracle Code Assist** | AI productivity, engineering culture |
| **Faraday retry/exponential backoff story** | Production reliability, resilience patterns |
| **API v1–v16 decommission with 6-month deprecation window** | API governance, managing technical debt with consumers |

---

## 9. Wednesday Logistics Checklist

- [ ] Reply to HR confirming attendance + ask for office address / floor / building
- [ ] Carry original government-issued ID proof
- [ ] Carry 2 printed copies of resume (one for Srikanth, one if second person joins)
- [ ] Arrive 10 minutes early — check in at reception, ask for Srikanth
- [ ] Phone on silent before entering the building
- [ ] Questions memorized — don't read from phone in the room
- [ ] Plan nothing tight for the hour after — final rounds can run long

---

*Built from: Round 1 transcript (full), Round 2 transcript (full), Files 01–03, 07 from master prep system, all previous coaching sessions.*
*Last updated: June 2026*
