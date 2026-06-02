# Optum OCM — Round 3 In-Person Prep
**Wednesday 12pm | Contact: Srikanth (Senior Director, same as Round 2)**

---

## 1. What to Expect

### Format
In-person final round at this level typically runs 60–90 minutes. Srikanth is your confirmed contact and primary interviewer. There may be one or two additional people — a peer Director, a Principal Engineer, or a TPO (Technical Product Owner). Treat every person in the room as a decision-maker.

### What each interviewer is assessing

| Who | Their lens | What they're really asking |
|---|---|---|
| **Srikanth (Senior Director)** | Executive presence, leadership maturity, fit | "Do I want to work with this person? Can I trust them with my programs?" |
| **Peer Director** (if present) | Cross-team collaboration, no-silo mindset | "Will this person compete with me or complement me?" |
| **Principal Engineer** (if present) | Technical credibility, engineering judgment | "Does he understand what good engineering looks like at scale?" |

### Tone to aim for
Not an interview — a **peer conversation**. Srikanth already knows your resume. He's assessing whether he wants to work *with* you. Be collaborative, ask questions, react to what he says. This round is yours to lose, not win from scratch.

---

## 2. Your Opening — Two Versions

### 30-second version (if asked "tell me about yourself" with limited time)
> "I lead engineering teams across healthcare and banking platforms — most recently at Oracle Cerner, where I ran Care Coordination products serving 500 million patient records across 120-plus customers. Before that I delivered a $5M Kubernetes modernization at Optum. I'm drawn to this role specifically because prior-auth is at exactly the intersection of platform scale and AI-driven clinical decision support — which is where I've been spending most of my energy these last two years."

### 60-second version (standard opening)
> "I'm a Senior Engineering Manager with 20-plus years across healthcare, banking, and insurance. At Oracle Cerner I led two Care Coordination products — a risk stratification engine and a care gap analytics suite — serving 120-plus customers and 500 million patient records. I ran a team of 14 engineers across three scrum teams, accountable end-to-end from backlog to production. Before that, at Optum under the EIP program, I led a $5M cloud-native Kubernetes migration, ramped four teams of freshers and laterals, and built engineering practices around GitHub Actions and automated pipelines.
>
> What's brought me to this conversation is a deliberate choice — I want to work on a platform where AI isn't a side project but a funded program with clinical stakes. The prior-auth problem, the 70-30 traditional-to-AI split you described, the behavioral and post-acute expansion — that's a specific combination of scale, domain complexity, and AI maturity that I haven't seen matched elsewhere. That's why I'm here."

> **Key change from Rounds 1 and 2:** Lead with *management shape* (team size, scrum teams, accountability) not just technical projects. End with *why this role specifically* — not generic enthusiasm. Srikanth already heard the Cerner and Kubernetes story; this version connects those facts to *his* programs.

---

## 3. Questions Srikanth Is Likely to Ask

### If Srikanth is the only interviewer

These are the questions most likely from a Senior Director in a final-round in-person, based on what he flagged in Round 2 (quality metrics, AI programs, team accountability) and standard hiring-manager closing questions.

---

**Q1: "What would you do differently in your first 90 days here compared to what you've done before?"**

*Why he asks:* He wants to know if you understand this role is different from Oracle Cerner — new team, new domain, new stack (Azure + React vs OCI + Java). He's also checking for self-awareness.

**Answer:**
> "The first 30 days I'd listen more than I act. I'd do 1:1s with every engineer and tech lead on the team — not to assess them, but to understand what's working, where they feel blocked, and what they're proud of. I'd shadow the sprint ceremonies without trying to change them immediately. And I'd read the last three post-mortems and the current tech-debt backlog — those two things tell me more about a team's real state than any architecture diagram.
>
> Days 30–60, I'd start building trust by removing one or two real blockers — things that are annoying the team but haven't been fixed. Quick wins that show I'm there to serve the team, not audit it.
>
> By day 90, I'd have a clear point of view on the one structural thing I'd change — whether that's a process, a dependency, a tooling gap, or an ownership boundary — and I'd bring that to you with data, not opinion. The mistake I've made before is forming that opinion in week two and acting on it in week three. This time I'd earn the credibility first."

---

**Q2: "Tell me about a time you had to make a decision your team disagreed with."**

*Why he asks:* He described a flat, accountable org. He wants to know you can make hard calls without consensus, and that you can hold the decision without becoming defensive.

**Answer (STAR):**
> "At Oracle Cerner, when we were planning the cloud migration from AWS to OCI, the team's strong preference was to lift-and-shift everything first and optimize later. The argument was speed — get off AWS, declare victory, then fix things. I disagreed. My assessment was that a lift-and-shift on our OpenSearch indexes would create a brittle(easy to break), expensive-to-operate state that we'd be stuck with for two-plus years.
>
> I made the call to do a parallel-run migration with weekly validation gates — keep AWS alive, run both environments, cut over customer by customer, shut down AWS only after 30 days of clean validation. The team pushed back hard — it was more work, more coordination, longer timeline.
>
> I explained my reasoning once clearly and didn't relitigate(debate again) it. I gave each lead ownership of their migration stream so they had real accountability, not just tasks. By the end, three of the five leads told me they'd changed their minds midway through when they saw the first two lift-and-shift attempts catch real issues in parallel.
>
> The outcome: zero customer-impacting incidents during migration. If we'd lifted and shifted, I'm confident we'd have had at least two P1s."

---

**Q3: "How do you manage engineering quality when you're under delivery pressure from product?"**

*Why he asks:* He flagged quality metrics as a current pain point in Round 2. He wants a specific answer, not a framework.

**Answer:**
> "I run a 20% sprint buffer that's protected for tech debt and quality work — not negotiable with product unless we're in a genuine P0 situation. The key is that I don't frame it as 'taking time away from features.' I frame it as 'this is what keeps our velocity predictable.' A team that skips quality work for three sprints doesn't go 20% faster — they go 20% faster for six weeks and then grind(push through) to a halt.
>
> For security specifically, I hold a hard line on CVSS 7-plus vulnerabilities — those go to the top of the backlog regardless of sprint plan, because the cost of a breach in a healthcare platform is not a tradeoff I can make for delivery speed.
>
> For larger migrations — Java version upgrades, Spring Boot, library consolidation — I scope and estimate them separately and bring them to PI planning as their own line items with business impact data: latency numbers, incident history, what it costs us to keep carrying the debt. When product sees 'this is causing three P2s per quarter and will cause a P1 within six months,' they stop treating it as optional.
>
> I've never had a product manager fight me on tech debt once I've shown them the cost of not doing it."

---

**Q4: "How do you think about building an AI team within an existing engineering org?"**

*Why he asks:* He told you AI is capital-funded and the differentiator this year. He wants to know you have a concrete model, not just enthusiasm.

**Answer:**
> "I think about it in two tracks. The first track is productivity AI — GitHub Copilot, code assist, prompt-structured test generation. This doesn't need a separate team; it's an engineering practice you embed across all your existing squads. The governance work here is around prompt standardization, manual PR review staying non-negotiable, and measuring the right things — not 'are people using it' but 'is code review time down, is test coverage up, are escaped defects trending down.'
>
> The second track is clinical AI — where the model output has patient impact. This needs different governance entirely. I'd structure it as a pod embedded within the product squad: an ML engineer, a data engineer, a domain SME, and a dedicated QA person focused on output validation. The key principle I hold is human-in-the-loop for any recommendation that a care manager or clinician acts on. The model can surface and rank; a human decides. That's not just a safety choice — in a CMS-regulated prior-auth environment, it's the only defensible architecture.
>
> What I've learned from the POC work at Cerner is that the hardest part isn't the model — it's grounding. If you don't have clean, consistent source-of-truth data that the model can reference, you get confident-sounding hallucinations, and in healthcare that's a clinical risk, not just a UX problem."

---

**Q5: "What's your leadership style, and how does it adapt?"**

*Why he asks:* Standard fit question, but at this level he's listening for self-awareness and range — can you lead both senior engineers who need autonomy and junior ones who need structure?

**Answer:**
> "I default to coaching over directing — I'd rather ask the right question than give the answer, because the person closest to the problem usually has a better answer than I do. But I adapt based on two variables: the engineer's experience level and the situation's urgency.
>
> With a senior engineer or tech lead, I set the outcome and stay out of the how. With someone newer, I stay closer — pair on design, review more frequently, give explicit feedback on the work, not just the result.
>
> In a P1 or high-stakes delivery crunch, I shift gears — I'm more directive, I make faster calls, I cut the debate shorter. Teams appreciate that clarity in a crisis even if they wouldn't want it every day.
>
> The failure mode I watch for in myself is over-coaching when a situation just needs a decision. I've had to learn to recognize when someone doesn't want a Socratic( asking questions to guide thinking, rather than giving answers directly) conversation — they want me to say 'do it this way' and move on."

---

### If a Peer Director joins

These questions are more likely from a peer Director assessing collaboration.

---

**Q6: "How do you handle a situation where another team's dependency is blocking your sprint?"**

**Answer:**
> "First I try to resolve it at the team level — direct conversation between my tech lead and their counterpart. Most dependency issues are coordination problems, not conflict, and they resolve at that level without escalation.
>
> If it doesn't resolve in 48 hours, I pick up the phone with the other EM directly — peer-to-peer, no hierarchy, 'here's the impact on my sprint, here's what I need, what do you need from me to unblock this?' In 80% of cases that's the end of it.
>
> If there's a genuine resource or priority conflict that neither of us can resolve, I escalate with data — not as a complaint but as a trade-off for leadership to make. 'Here are the two programs in conflict, here's the cost of each path.' I don't ask leadership to solve it for me; I ask them to make the priority call and I execute it.
>
> What I try never to do is let a dependency sit and quietly slip the sprint without flagging it early. The damage from a late flag is always worse than the awkwardness of an early one."

---

**Q7: "How do you share learnings or engineering practices across teams?"**

**Answer:**
> "I run a fortnightly engineering sync — not a status meeting, a practice-sharing session. Each sprint, one team brings something: a post-mortem, a design decision they made, a tool they adopted. It's 30 minutes, attendance is expected, and it builds a shared vocabulary across teams.
>
> For standards that need to be consistent — security practices, API design, PR review norms, AI tooling governance — I document them in a lightweight decision record and make them part of onboarding. Not a wiki page no one reads — a `.md` file that's referenced in the repo and reviewed in code review.
>
> What I've found is that engineers resist top-down standards but adopt peer-driven ones. So I make sure the person presenting the practice is the engineer who built it, not me. I facilitate; they own."

---

### If a Principal Engineer joins

These will be more technically pointed — not coding, but engineering judgment.

---

**Q8: "How do you make architectural decisions? Walk me through your process."**

**Answer:**
> "I start by separating the decision from the conversation. Before I go into any architecture discussion, I want to know: is this a two-way door or a one-way door? Reversible decisions I push down to the tech lead and trust their judgment. Irreversible or expensive-to-change decisions I get involved in directly.
>
> For a real architecture decision — say, choosing between an event-driven and a request-response pattern for a new integration — I ask the tech lead to bring three things: the options they considered, the trade-offs they see, and their recommendation with a reason. I'm not there to override them; I'm there to stress-test the reasoning and make sure they've thought about operational cost, not just build cost.
>
> Things I specifically ask about that engineers often underweight: what does failure look like, how do we observe this in production, and what's the mitigation path if we get this wrong? If those three questions don't have good answers, the decision isn't ready.
>
> Simple way of checkin above questions: What could go wrong, how will we see it happening in production, and what is our backup plan if it fails?
>
>This is a Principal Engineer type question testing whether you've thought about risk and rollback before starting a change. Your answer for this context (cloud migration, AI features, Java upgrades) should always cover three things: how you detect failure early (monitoring, alerts, error rates), how you limit the blast radius (feature flags, canary releases, phased rollout), and what the rollback plan is (DNS cutover reversal, parallel run, version pinning).
>
> I try to document the decision in an ADR(Architecture Decision Record) — not a long document, just the context, the options considered, the decision, and the consequences. Six months later, when someone asks 'why did we do it this way,' the answer exists."

---

**Q9: "How do you handle technical debt in a fast-moving product team?"**

*(Likely from a Principal Engineer who has lived with bad debt decisions.)*

**Answer:**
> "I treat tech debt like a backlog item with a business cost, not a technical complaint. If I can't quantify the cost of the debt — slower delivery, higher incident rate, harder onboarding, security exposure — I can't prioritize it against feature work, and I shouldn't expect product to accept it on faith.
>
> My operating model: 20% buffer in every sprint for debt and quality. Hard mandatory fix for CVSS 7-plus vulnerabilities — no negotiation. For bigger structural work (runtime upgrades, library consolidation, re-architecture), I scope it separately and present it at PI planning as its own initiative with a cost-of-delay argument.
>
> The thing I've learned is that a team that never pays down debt doesn't slow gradually — it falls off a cliff. I've seen it twice: a team that deferred Spring Boot upgrades for 18 months and then spent a full quarter firefighting compatibility issues when a dependency EOLed(End of Life). That's the story I tell product when they push back on debt time."

---

**Q9: "Your React / Frontend Story?"**
>
> My frontend exposure has been at the integration level rather than deep development. In our Care Management and Readmission Prevention products, we had React-based UI components built as pluggable modules inside the Cerner MPages framework — so they lived inside a clinical workstation shell. My involvement was in making sure those components integrated cleanly with our backend APIs and that the data contracts were right. Direct enhancements to the React components were limited, so I won't claim deep React expertise. What I do bring is a clear understanding of the frontend-backend contract, API design for UI consumption, and how to work effectively with frontend engineers to define what they need.
>

**Q9: "Java 11 vs Java 17 vs Java 21 — what you need to know"** 
> 
> Java 11 (LTS, released 2018 — EOLed for free Oracle support)
> The version most enterprise teams were on. Introduced the var keyword for local variables, HTTP Client API, and cleaned up older APIs. Most teams stayed here because it was stable and supported.
> Java 17 (LTS, released 2021 — current mainstream enterprise standard)
> The version most teams are migrating to now. Key additions:
> 
> Sealed classes — you control which classes can extend yours. Good for domain modeling (e.g. a ClinicalEvent that can only be Admission, Discharge, or Transfer)
> 
> Records — a clean, short way to write immutable data classes. Instead of a POJO with constructor, getters, equals, hashCode — just record Patient(String id, String name) {}. Very useful for DTOs.
> 
> Pattern matching for instanceof — instead of if (obj instanceof String) { String s = (String) obj; } you write if (obj instanceof String s). Less boilerplate.
> 
> Text blocks — multi-line strings with """. Good for JSON templates, SQL in tests.
> 
> Strong encapsulation of JDK internals — some older libraries that used internal APIs break. This is the main migration pain point.

> Java 21 (LTS, released 2023 — forward-looking)
> The biggest addition is Virtual Threads (Project Loom). Traditional Java threads are OS threads — expensive, limited. Virtual threads are lightweight threads managed by the JVM, potentially millions of them. For a high-throughput system like prior-auth (70–80k requests/day) with lots of I/O waiting (DB calls, external API calls), virtual threads can significantly improve throughput without changing your code much. This is the main reason teams are starting to look at Java 21.
> Your story for the migration question:
> 
> "We were on Java 11 and planned a phased move to Java 17 as part of our platform modernization. The main work was identifying libraries that depended on internal JDK APIs that Java 17 now restricts — we used the jdeps tool to scan for those before touching any code. We also used the migration as an opportunity to refactor key DTOs into Records and clean up boilerplate. We planned it as a separate sprint item with its own estimated effort, presented at PI planning, rather than mixing it into feature work — because mixing migrations with features makes rollback very hard. Java 21 is on the radar specifically for Virtual Threads, which would benefit our high-I/O patient data processing, but we haven't committed to that timeline yet."

## 4. Your Questions for Srikanth

These are the most important questions — they signal you've been paying attention across all three conversations and you're thinking about *his* problems, not just the job mechanics. Pick 2–3 based on how the conversation flows.

**On the AI programs (highest priority — he said this is the differentiator this year):**
> "You mentioned AI has dedicated capital this year. How are you thinking about the boundary between productivity AI for the engineering team and clinical AI in the product — are those governed the same way, or do you see them as fundamentally different?"

**On the expansion roadmap:**
> "With behavioral health and post-acute care onboarding to the platform — what's the biggest engineering challenge in supporting multiple clinical lines of business on the same prior-auth core? Is it the data model, the tenancy architecture, or something else?"

**On team health and current gaps:**
> "What's the one thing the team needs most from this hire that isn't obvious from the job description?"

**On what success looks like** (use this if you haven't asked a version already):
> "If we're sitting here 12 months from now and this has worked out really well — what does that look like from your perspective?"
>
> Simple way: What does success look like for you in this role after one year?

**On the accountability model he described:**
> "You described the org as flat and accountable — teams own their outcomes end-to-end. How does that work in practice when a team hits a genuine dependency on another team's roadmap?"

---

## 5. Key Things to NOT Do

- **Don't repeat your Round 2 stories verbatim.** He's heard the readmission-risk POC, the Kubernetes migration, the availability answer. Build on them, don't replay them.
- **Don't use the word "DBA"** when describing the care manager query use case. You said "care manager with the role of DBA" in Round 2 — that confused the listener. Say "a care manager with a panel of patients" instead.
- **Don't give framework answers** when he asks behavioral questions. He wants a specific story: situation, your decision, the outcome. Not "my approach to tech debt is..." — give him the *incident*.
- **Don't leave without asking at least two questions.** In Round 2 you said "No" when he first opened the floor for questions. This time engage early.
- **Don't undersell the management scope.** When numbers come up, lead with people and teams: "14 engineers across three scrum teams" before you say "$5M migration."

---

## 6. Your Strongest Cards — Use These

| Card | When to play it |
|---|---|
| **$5M Kubernetes migration at Optum** | When he asks about delivery credibility or cloud-native experience |
| **120+ customers, 500M records at Cerner** | When scope and scale of accountability comes up |
| **Conversational UI / RAG POC with human-in-the-loop** | When AI governance or clinical AI comes up — this is your most differentiated story |
| **20% sprint buffer + CVSS 7+ mandate** | When quality vs. delivery pressure comes up |
| **30-day notice / immediate joiner** | Already played in Round 2 — don't repeat unless he brings it up |
| **Four-team ramp at Optum EIP** | When team-building or scaling engineering capacity comes up |

---

## 7. Logistics Checklist for Wednesday

- [ ] Reply to HR confirming attendance and asking for office address / floor details
- [ ] Carry original ID proof (government-issued)
- [ ] Carry 2 printed copies of your resume (one for Srikanth, one if a second person joins)
- [ ] Arrive 10 minutes early — check in at reception, ask for Srikanth
- [ ] Phone on silent before entering the building
- [ ] Have your questions memorized — don't read from a phone in the room

---

*Built from: Optum OCM Round 1 transcript, Round 2 transcript, 20+ rounds of interview prep history across Availity, Deloitte, Cubic, HighRadius, Wells Fargo, TJX, and Evernorth.*
