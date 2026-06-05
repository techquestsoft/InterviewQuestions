# Globalogic — Round 3 Prep
**Role: Senior Engineering Leader — 100 people, 8-10 client boards, end-to-end product engineering**
**Built from: Round 1 (Biju) + Round 2 (Fazeq) — every question covered**

---

> **What Round 3 will be:** Both Biju and Fazeq signalled more senior interviewers ahead.
> Biju said "at least three more rounds." Fazeq said "there will be various discussions."
> Expect a delivery leader or CTO-level person probing: AI delivery lifecycle, large-team
> leadership, client engagement at board level, and deeper technical architecture.
> The two corrections Fazeq made in Round 2 (LLM tool invocation + data containment)
> WILL be re-tested. Be ready.

---

## PART 1 — TOPICS COVERED IN ROUNDS 1 & 2 (CLEANED UP ANSWERS)

---

### 1. Introduction — right version for a 100-person role

**What went wrong:** Round 1 led with 20-person team and 6-person product. Round 2 said "20 years" (Round 1 said 22). Migration direction mangled in Round 1.

**Fixed facts to memorize:**
- Migration direction: **OpenShift → Kubernetes** (never the other way)
- Optum team: **30-plus engineers, 7 scrum teams**
- Cerner team: **20-plus engineers, 4 scrum teams**
- Years of experience: **22** (consistent, always)

**Clean 60-second version:**
> "I am Rajasekhar, I go by Shekhar. I have 22 years of experience across healthcare, banking, and insurance. Most recently I was Senior Engineering Manager at Oracle Cerner, leading four care coordination products with 20-plus engineers — readmission prevention, care management, care plan, and risk assessment — serving 120-plus healthcare customers and 500 million patient records.
>
> Before that, at Optum under the EIP modernization program, I led 30-plus engineers across seven scrum teams to migrate 110-plus microservices from OpenShift to Kubernetes, delivering $5M in annual infrastructure savings and significantly improving deployment velocity.
>
> In the last 18 months my focus has been at the intersection of cloud migration, ML model modernization, and AI-assisted engineering — directly aligned with what Globalogic is looking for."

---

### 2. Product deep-dive — what the interviewer actually wanted

**What went wrong:** 6-minute architecture walkthrough. They wanted your leadership role and what changed because of you.

**Clean 90-second version:**
> "The readmission prevention product computes risk scores for patients — low, medium, high — based on clinical data, and surfaces those scores to care managers so they can intervene before readmission. The team for this product is 6 engineers.
>
> In the last 12 months, my two biggest contributions: first, I led the migration of the data pipeline from AWS to OCI — coordinating with the HDI platform team, parallel-run with weekly validation gates, 120 customers cut over with zero incidents. Second, I drove the upgrade from a rule-based sequential scoring model to an ML-driven model — partnering with the data science team on integration, coordinating rollout. Accuracy improved from around 40% to 70-80%, directly improving client NPS."

---

### 3. PI Planning and commitment — keep this answer

> "We run a 6-day PI planning cadence across care coordination. Each team takes 1.5 days. We capture all features as EPICs in JIRA, rank them with the product team by business priority, calculate team bandwidth accounting for holidays, DevOps rotation, and a mandatory 20% buffer for tech debt — and commit only to what fits within that capacity. One engineer always rotates on DevOps support, so production incident capacity is already removed before any feature commitment is made."

---

### 4. Engineering metrics — use DORA vocabulary

**What went wrong:** You knew the concepts but not the industry terms. Formula was given as example before the formula itself.

**Formula first, always:**

| DORA Metric | Formula | Your target |
|---|---|---|
| **Deployment Frequency** | Releases per time period | Sprint-level (2 weeks) |
| **Lead Time for Changes** | Code commit → production | Trending down quarter-on-quarter |
| **Change Failure Rate** | (Failed/reverted releases ÷ Total releases) × 100 | Below 5% |
| **MTTR** | Average time to restore after incident | Under 30 min for P1 |

**Spoken version:**
> "I track four DORA metrics: deployment frequency, lead time for changes, change failure rate — failed releases divided by total releases, target below 5% — and MTTR. Beyond DORA, I track business-impact KPIs: risk scoring accuracy, care manager productivity. The Faraday P1 incident we resolved in 30 minutes — that's a real MTTR data point."

---

### 5. Productivity acceleration — 500 to 800 story points

**What went wrong:** Rambled into velocity manipulation before reaching AI. AI agent answer needed two prompts.

**Lead with the full AI pipeline immediately:**
> "Three levers. First, AI-assisted development across the full SDLC — code generation, test generation, documentation, legacy code comprehension. At Cerner, Oracle Code Assist gave us roughly 20% productivity improvement over 15 months. Second, AI agent for PR review embedded in the pipeline — the agent reviews every PR against coding standards, security patterns, and test coverage rules, approves routine changes, and escalates complex or security-sensitive changes to humans. Manual review is reserved for the 20% that genuinely needs it. Third, smarter feature decomposition — identify the 20% of a feature that delivers 80% of the value, ship that first. MVP mindset applied systematically shortens lead time without cutting scope permanently."

---

### 6. AI automated code review — must be first answer, not rescued answer

> "Build an AI agent into the PR pipeline. The agent is configured with coding standards, security patterns, test coverage requirements, and common anti-patterns specific to our codebase. Every PR — AI-generated or human-written — goes through the agent first. The agent either approves for merge, flags specific issues for the engineer to fix, or escalates to a senior engineer for manual review. The escalation logic is the key: routine, well-formed code goes fast. Changes touching security-sensitive paths, core business logic, or data contracts always get human eyes. This reserves manual senior review for the 20% of PRs that genuinely need judgment."

---

### 7. Scrum team size — defending 4-member migration teams

> "For a pure technical migration like OpenShift to Kubernetes, 4 engineers per domain-scoped team is right. The work is well-defined — migrate these microservices using an established template. Domains are separate so teams don't block each other. Scrum master and product owner span multiple teams. For a production feature team, 6 to 7 is correct — that needs QA, front-end, back-end, and DevOps coverage within the team."

---

### 8. S3 security — complete answer, all six layers, unprompted

**What went wrong:** Fazeq had to prompt you for retention. Subnet answer was challenged. VPC endpoint was missing.

**Complete answer:**
> "Six layers for S3 in a HIPAA environment.
>
> One — encryption at rest, AES-256 server-side encryption. Two — encryption in transit, TLS for all data movement. Three — RBAC and ABAC through IAM policies — only authorized system accounts per tenant access their specific bucket. Four — VPC endpoint for S3 — S3 traffic stays within the VPC, never traverses the public internet even within AWS. This is the correct network-level control, not subnets. Five — data masking and anonymization before cross-team sharing — PII fields like SSN masked, PHI anonymized before HDI AI team processing, satisfying HIPAA Safe Harbor de-identification. Six — retention policy with automated deletion — data retained for contract period, automatically deleted by batch job after expiry, with audit logging of every deletion event."

**Masking vs anonymization — be precise:**
> "Masking replaces sensitive values with symbols — SSN becomes *** — structure remains, potentially reversible. Anonymization is irreversible — the individual cannot be re-identified. HIPAA Safe Harbor requires anonymization. We use masking for internal debugging where reversibility is needed, anonymization for external vendor sharing."

---

### 9. Monolith to microservices — name the pattern

**What went wrong:** Described the concept correctly but never named it.

**Use the term Strangler Fig:**
> "The industry-standard approach is the Strangler Fig pattern — you don't rewrite the monolith, you gradually strangle it. Build new microservices alongside the monolith, route specific traffic to them, expand coverage incrementally until the monolith handles less and less, then retire it. The business never stops.
>
> For a dense 10-year monolith where database separation is impractical, I'd apply it at the service layer first — expose stateless APIs on top of the existing database without touching the database yet. Those APIs can be deployed and scaled independently. Then over time, peel the database apart domain by domain. Slower than a big-bang rewrite, but each piece is validated in production before proceeding."

---

### 10. Cloud migration — name specific services

**What went wrong:** Correct thinking but no specific database services named.

> "Four-layer assessment — data, data access, gateway/security, front-end — migrate phase by phase, one subdomain at a time.
>
> Database selection: transactional relational workloads → AWS RDS or Aurora. High-throughput unstructured data → DynamoDB or DocumentDB. Search and analytics → OpenSearch Service. Choice driven by throughput requirements, query patterns, and cost at scale.
>
> Application layer: cloud-agnostic through infrastructure-as-code — Terraform or Helm charts — so moving from AWS to OCI or Azure is a configuration change, not a rewrite. We proved this on the Cerner AWS-to-OCI migration."

---

## PART 2 — AI DELIVERY LIFECYCLE (BIJU EXPLICITLY TOLD YOU TO PREPARE THIS)

---

### 11. What is the AI Delivery Lifecycle?

**Memory Hook:** Plan → Data → Model → Integrate → Govern → Operate → Improve

> "Traditional SDLC covers requirements through deployment. AI DLC extends that with phases specific to model development and governance.
>
> **Plan** — define the AI use case, success metrics, data requirements, build-vs-buy decision. Most teams skip this. That's why AI projects fail to deliver value.
>
> **Data** — discovery, quality assessment, labeling, pipeline setup. In healthcare this includes de-identification, consent, HIPAA compliance. Data phase is typically 40% of total effort.
>
> **Model** — select or fine-tune the model, validate, measure accuracy against baseline. For most enterprise use cases, this means selecting a foundation model and doing RAG or fine-tuning — not training from scratch.
>
> **Integrate** — embed the model into the product. API design, tool configuration for agentic flows, UI integration, latency testing, safety testing.
>
> **Govern** — security review, compliance review, bias testing, explainability, audit logging, human-in-the-loop design. In regulated industries this is a gate, not optional.
>
> **Operate** — deploy, monitor model performance, track cost per query, alert on drift or accuracy degradation, manage LLM provider SLAs.
>
> **Improve** — feedback loops from production usage, retraining triggers, prompt optimization, model upgrades. AI systems degrade over time without active maintenance — this is model drift."

---

### 12. LLM tool invocation — the corrected answer (Fazeq corrected you on this)

**Never say:** "LLM cannot invoke tools" or "LLM only takes text and gives text."

**This must be a reflex:**
> "Modern LLMs support function calling and tool use natively. You define tools as JSON schemas — name, description, parameters. When the LLM receives a user query, it reads the tool descriptions and decides which tool to invoke and with what parameters. The host application executes the tool call, gets the result, passes it back to the LLM, which then generates the final response. The LLM selects the tool and provides parameters — the orchestration layer executes it. This is the foundation of agentic AI."

---

### 13. LLM data containment — the corrected answer (Fazeq corrected you on this)

**What went wrong:** You said system prompting contains the data. Fazeq told you the correct answer: install the LLM in your infrastructure.

**Lead with infrastructure, always:**
> "Data containment has three layers, in order of strength.
>
> First and strongest — infrastructure containment. Deploy the LLM within your own cloud boundary. On Azure: Azure OpenAI Service with private endpoints — traffic never reaches the public OpenAI API. On OCI: Oracle GenAI within the OCI boundary. On AWS: Amazon Bedrock with VPC endpoints. When the model runs inside your infrastructure, data physically cannot leave. This is the only truly reliable technical containment.
>
> Second — system prompting and grounding. Instruct the model to answer only from retrieved context, not general knowledge. This is a behavioral control — the model follows the instruction but it is not a technical guarantee. A model can sometimes deviate.
>
> Third — contractual controls. Vendor agreements that your data won't be used for model training. Legal, not technical.
>
> In a HIPAA environment, all three layers are required. Infrastructure containment is non-negotiable — you cannot rely on system prompting alone."

---

### 14. Agentic AI — how it works end-to-end

**Memory Hook:** User Query → Orchestrator → LLM selects tool → Host executes → Result back → Response

> "An agent is an LLM-powered system that takes multi-step actions to complete a goal. Components:
>
> The orchestrator — LangChain or LangGraph — manages conversation state, tool registry, and execution loop. The LLM receives the user query and tool definitions, reasons about what needs to happen, emits a tool-use instruction with parameters. The host application executes the tool — reads from OpenSearch, queries a database, calls an API. Result comes back to the LLM, which either formulates the final answer or decides it needs another tool call.
>
> Healthcare example: query is 'top 10 high-readmission-risk patients not contacted this week.' Agent calls OpenSearch tool for risk scores, then calls contact-history tool to filter, then returns a formatted list. Multiple tool calls, chained automatically.
>
> Governance: RBAC on every tool — agent can only access data the user is authorized for. Read-only by default — write actions require explicit approval. Human-in-the-loop for irreversible actions. Full audit log of every tool call."

---

### 15. AI across the full SDLC — story to deployment

Biju said: *"These days a lot of things are automated from story to deployment."*

> "AI touches every stage now:
>
> **Story creation** — LLM generates user stories from product briefs. PM reviews. Saves 2-3 hours per feature.
> **Design** — LLM generates first-draft architecture from requirements. Architect critiques rather than creates from blank page. 30-40% time saving.
> **Code generation** — developer writes the spec, AI generates the implementation. Developer reviews and corrects.
> **Test generation** — AI generates unit tests from function signatures. Coverage baseline established before feature code is written.
> **Code review** — AI agent reviews PR, flags issues, approves routine changes, escalates complex or security-sensitive to humans.
> **Documentation** — AI generates API docs, runbooks, ADRs from code and decisions.
> **Deployment** — AI monitors deployment health in real time, identifies anomalies, suggests rollback if error rates spike.
>
> Governance principle across all stages: AI accelerates, humans own. No stage is fully autonomous — every AI output has a human checkpoint appropriate to the risk level."

---

### 16. How do you build an AI DLC practice in a new client engagement?

**Likely question given 8-10 client board scope:**

> "Four steps.
>
> First, assess the current state — what AI tools are the client's teams already using? What data assets exist? What governance is in place? You can't impose AI DLC on a team without basic CI/CD hygiene first.
>
> Second, start with productivity AI, not product AI. Get engineers using AI for code generation and review within the first 30 days. Measure before and after. This builds confidence, surfaces governance gaps, and gives real data to show the client.
>
> Third, run a focused product AI POC on one high-value, low-risk use case. Define the success metric before starting. Run in shadow mode — AI runs in parallel, outputs logged internally, nothing reaches end users until accuracy is validated.
>
> Fourth, build governance in parallel with the POC — not after. Data security review, model approval, bias testing, audit logging, human-in-the-loop design. In a regulated client environment, governance is a launch gate. If you build it after, you delay by months."

---

## PART 3 — LARGE TEAM LEADERSHIP (100 PEOPLE, 8-10 CLIENT BOARDS)

---

### 17. How do you manage 100 people across 8-10 clients?

**Memory Hook:** Layer the structure → Empower leads → Common playbook → Data-driven oversight → Clear escalation

> "You can't manage 100 people directly — you manage the leaders. My structure: five to six engineering managers or tech leads, each owning 15-20 engineers across one to two client boards. I own the managers, cross-client engineering practices, client executive relationships, and the escalation path.
>
> What keeps it coherent: a common engineering playbook — same sprint cadence, same quality gates, same incident management process — adapted where the client has specific requirements. Engineers can move between client teams without losing context.
>
> Oversight without micromanagement: weekly metrics review across all teams — deployment frequency, change failure rate, MTTR, customer satisfaction — not to interrogate but to spot outliers early. If one team's change failure rate spikes, I'm in that conversation the same week.
>
> Client relationships: I am the executive point of contact for each client, supported by the team lead for that engagement. Clients feel they have a senior leader accountable to them."

---

### 18. Client escalation at board level

> "Three things simultaneously: contain, communicate, fix — in parallel.
>
> Contain — understand blast radius immediately. How many customers affected? Getting worse or stabilizing? Do we need to take anything offline?
>
> Communicate — the client's executive hears from me, not from a ticket update. Within 30 minutes of a P1 I'm on the phone with the client delivery leader. I give them what I know, what I don't know, and when they'll hear next. I never go silent — silence in a crisis destroys trust faster than anything else.
>
> Fix — I assign one clear incident commander who owns the war room, makes decisions, updates me every 15 minutes. I don't try to be both client communicator and incident commander simultaneously.
>
> After resolution: written post-mortem within 48 hours, shared with the client — what happened, why, what changed. A client who sees a rigorous post-mortem leaves more confident than before the incident."

---

### 19. Engineering excellence across a 100-person team

> "Three things that scale.
>
> A shared engineering playbook — documented standards for code quality, API design, security, incident response, AI tool usage. Enforced in CI/CD gates, referenced in onboarding. Engineers know what good looks like because it's written down.
>
> A fortnightly engineering community of practice — one team presents something real each time: a post-mortem, an architectural decision, a tool adopted. Cross-pollination across client teams. Engineers learn from each other faster than from any training.
>
> Recognition tied to engineering quality, not just delivery speed. If only hitting the sprint target is celebrated, engineers optimize for the sprint target. If post-mortems are celebrated as learning, engineers write good post-mortems. Culture follows incentives."

---

### 20. MBRs and QBRs — you need to know these for 8-10 client boards

**What went wrong:** Fazeq asked and you said "we don't do much of that." For this role, MBRs and QBRs are standard.

**MBR (Monthly Business Review):**
> "Monthly touchpoint with client delivery and business leadership — 60 minutes. I'd cover: delivery health (committed vs delivered, variance and reason), quality metrics (defect rates, incident count and MTTR, change failure rate), upcoming risks and dependencies for the next month, and one business impact highlight. The goal: the client never has a surprise. Everything in the MBR they've heard during the month — the MBR is the structured summary."

**QBR (Quarterly Business Review):**
> "Deeper strategic review — 2 hours with senior client leadership including business stakeholders. I'd cover: quarterly delivery scorecard vs targets, roadmap progress, tech debt and platform health, talent and team updates, and forward-looking roadmap for next quarter. I'd also present AI productivity data — what AI tools are in use, measurable productivity gains, what we're planning next. For a senior client, the QBR is how they justify their Globalogic investment to their own leadership."

---

## PART 4 — QUESTIONS TO ASK IN ROUND 3

> Ask at least 2. Ask things only a senior person can answer from experience.

- **On management structure:** "When you say 8-10 boards — is this one engineering leader managing all clients directly, or is there a layer of team leads reporting to me? I want to understand the structure before I walk in."
- **On AI expectation:** "You've mentioned AI flavor is important. Is this primarily AI-assisted engineering productivity for the teams, or is the client expecting AI-native product features? Or both?"
- **On current challenges:** "What's the hardest thing about this engagement right now — delivery, client relationship, talent, or something else? I want to know what I'd be walking into."
- **On success:** "What does 12-month success look like — how will you know this was the right hire?"

---

## PART 5 — THINGS NOT TO DO IN ROUND 3

- **Don't say "LLM cannot perform any action"** — corrected in Round 2. Reflex: LLMs support tool use via function calling.
- **Don't say "system prompting contains the data"** — Fazeq corrected this. Lead with infrastructure containment.
- **Don't say "Kubernetes to OpenShift to Kubernetes"** — direction is OpenShift → Kubernetes.
- **Don't give architecture walkthrough when asked about your role** — lead with what you led and what changed.
- **Don't give an example when asked for a formula** — formula first, example optionally after.
- **Don't say "we don't do MBRs much"** — understand the format, show you can run them.
- **Don't say "20 years" then "22 years"** — pick 22, use it consistently.
- **Don't leave security incomplete** — all six layers unprompted: encryption at rest, in transit, RBAC/ABAC, VPC endpoint, masking/anonymization, retention policy.
- **Don't ramble into velocity and attrition** before reaching AI tools when asked about productivity.

---

## PART 6 — KEY TERMS

| Term | Simple version |
|---|---|
| **AI DLC** | AI Delivery Lifecycle — full process from AI use case planning through model deployment and ongoing improvement |
| **Strangler Fig pattern** | Gradually replace a monolith by building new services alongside it, routing traffic incrementally until monolith is retired |
| **Function calling / Tool use** | LLM capability to select and invoke predefined tools — the foundation of agentic AI |
| **Model drift** | Model accuracy degrades over time because real-world data has changed from training data |
| **Shadow mode** | AI runs in parallel with existing system — outputs logged internally, nothing affects real users — used for validation before go-live |
| **DORA metrics** | Deployment Frequency, Lead Time for Changes, Change Failure Rate, MTTR |
| **VPC endpoint for S3** | Network control keeping S3 traffic inside your VPC — never traverses public internet — correct AWS network security answer |
| **Anonymization vs masking** | Anonymization irreversible (HIPAA Safe Harbor). Masking replaces values with symbols, reversible. |
| **MBR** | Monthly Business Review — monthly delivery health review with client leadership |
| **QBR** | Quarterly Business Review — strategic review with senior client stakeholders |
| **RAG** | Retrieval-Augmented Generation — model retrieves verified documents first, generates only from those |
| **LangChain / LangGraph** | Orchestration frameworks for LLM-powered applications and multi-step agentic workflows |

---

## PART 7 — STRONGEST CARDS FOR ROUND 3

| Card | When to play it |
|---|---|
| **110 microservices OpenShift→Kubernetes, $5M savings** | Large-scale delivery, cloud migration, platform modernization |
| **120 customers, 500M patient records** | Scale, multi-tenant engineering, accountability |
| **AWS→OCI parallel-run migration, zero incidents** | Risk management, client trust, cloud expertise |
| **ML model 40%→80% accuracy, cross-team coordination** | AI product delivery, partnering with data science |
| **Oracle Code Assist, ~20% productivity, 15 months** | AI DLC, engineering productivity, measurable ROI |
| **Conversational UI / RAG POC design** | Agentic AI, clinical AI architecture, AI product vision |
| **Faraday P1 — 30-min resolution, 15-min comms cadence** | Incident management, client trust, large-scale reliability |
| **20% sprint buffer + CVSS 7+ mandate** | Quality under delivery pressure, engineering discipline |

---

*Built from: Globalogic Round 1 (Biju) full transcript, Round 2 (Fazeq) full transcript, GenAI prep file 07.*
*Last updated: June 2026*
