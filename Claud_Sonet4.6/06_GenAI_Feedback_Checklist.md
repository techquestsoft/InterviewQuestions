# Interview Prep — File 6 of 6
# GenAI / Agentic AI + Company-by-Company Feedback + Master Checklist

---

## SECTION A — GenAI & Agentic AI

> **Rule:** Be honest about what is in production vs what is in design. Interviewers catch inflated claims immediately.
> **Rule:** Your GenAI architecture diagram is your strongest asset — refer to it when describing the conversational AI use case.

---

### Q1: How are you using GenAI currently?

> "At two levels.
>
> For engineering productivity: Oracle Code Assist — Claude as the underlying LLM — is installed for every engineer. We use structured prompting templates, not free-form queries. Oracle's dashboard tracks weekly usage per engineer. After 15 months I see 15–20% improvement in sprint velocity at team level. Individually, I track story point completion before and after — engineers who use the tool consistently take on more complex stories over time.
>
> For product use case: I have proposed and designed a conversational AI layer for our Care Management front end. Today, a care manager searches for one patient at a time to see their readmission risk. The use case I am designing would let them ask in natural language: 'Show me the top 10 patients with highest readmission risk this week.' The system would translate that to an OpenSearch query, retrieve results, and return a formatted response. This is currently at proposal stage with HDI product leadership — I am being transparent about that."

---

### Q2: Describe your GenAI architecture end-to-end

**Memory Hook:** Access → Orchestrate → Retrieve → LLM → Guard → Observe

**Architecture (based on the diagram you have):**

```
Care Manager (User)
    │ Natural language query: "Top 10 readmission patients this week"
    ▼
Chat UI (React + WebSocket)
    Captures query, renders structured results
    │
    ▼
API Gateway / BFF (Spring Boot / FastAPI)
    Auth, rate limiting, role validation
    │
    ▼
Conversational AI Service (Python + LangChain)
    Orchestrates the flow, decision logic
    │
    ├──► Memory Store (Redis)
    │       Conversation context (multi-turn)
    │       Cached patient data for follow-up queries
    │
    ├──► Intent & Planner (LLM + Function Calling)
    │       Understands query intent
    │       Selects which tool to call
    │
    └──► Tool Layer
            │
            ├──► OpenSearch Tool
            │       Fetches patient records by risk score
            │       Filter, sort, aggregate
            │
            └──► RAG Tool (for clinical knowledge)
                    Vector DB (FAISS / Pinecone)
                    Retrieves clinical guidelines, drug interactions
                    Explains WHY a patient is high risk
    │
    ▼
Response Generator (GPT-4 / Azure OpenAI)
    Generates natural language response from retrieved data
    │
    ▼
UI Renderer
    Tables, charts, risk explanations
```

**When to use OpenSearch Tool vs RAG Tool:**
- OpenSearch: structured patient data queries — "top 10 patients," "filter by hospital," "sort by risk score"
- RAG: unstructured clinical knowledge — "why is this medication increasing readmission risk," "what are the clinical guidelines for this condition"

---

### Q3: RAG vs Direct Query — when do you choose each?

**Memory Hook:** Structured → Direct | Unstructured → RAG | Multi-step → LangGraph

| Scenario | Approach | Reasoning |
|----------|---------|-----------|
| "Top 10 readmission patients" | Direct OpenSearch query | Structured data, deterministic, low latency |
| "What medications reduce readmission for CHF patients?" | RAG with vector DB | Unstructured clinical docs, semantic search needed |
| "Show top 10 patients, then filter by hospital, then show their medications" | LangGraph orchestration | Multi-step, each step's output feeds next step |

> "For our readmission use case, OpenSearch's native K-NN vector search is sufficient for the initial retrieval — I do not need a separate RAG pipeline or external vector database for structured patient queries. RAG becomes relevant when we want the LLM to answer questions from unstructured clinical notes or guidelines stored as documents. That is Phase 2."

---

### Q4: How do you handle hallucinations in GenAI systems?

**Memory Hook:** Ground → Constrain → Validate → Loop

> "Four mechanisms.
>
> Grounding via retrieval: the LLM answers only from retrieved data, not from general knowledge. The prompt explicitly says 'answer only using the following patient records' — not 'use your knowledge.'
>
> Prompt constraints: the system prompt defines the scope, format, and what the model must not do. For a clinical system: 'Do not recommend specific medications. Do not make diagnostic statements. Only summarize what is in the retrieved records.'
>
> Output validation: I validate the structure of the response before displaying it. If the response format does not match the expected schema, it is rejected and the user gets a graceful error.
>
> Human-in-the-loop: for high-stakes decisions like clinical recommendations, the LLM response is presented as context for the care manager's judgment — not as a directive. The care manager decides. The AI informs."

---

### Q5: How do you build guardrails for GenAI in an engineering team?

**Memory Hook:** Approved Tools → Structured Prompting → CI Gates → Measurement

> "Three levels of guardrails.
>
> Approved tooling: company-approved AI tools only — no personal ChatGPT accounts, no external tools that might expose patient data. Oracle Code Assist stays within Oracle's cloud boundary.
>
> Structured prompting standards: I define prompting templates for common engineering tasks — new feature scaffolding, test generation, code review, documentation. Engineers do not free-form prompt. The templates embed our coding standards and security patterns.
>
> CI pipeline gates: AI-generated code is not exempt from code review or coverage requirements. I am looking at adding a step that flags large blocks of code that pattern-match to common AI outputs — high cyclomatic complexity with no exception handling, test files with only happy-path cases. These get mandatory senior review.
>
> Measurement: I track AI-assisted code review pass rate on first submission versus rework rate. If AI-assisted code escapes more defects than manually coded features, the guardrails are not working and I tighten them."

---

### Q6: How do you measure GenAI success?

**Memory Hook:** Efficiency → Quality → Business Impact

**Three KPIs:**

> "One — Feature Delivery Velocity: time from feature sign-off to production deployment, compared to pre-AI baseline. Target: 20% reduction in 6 months.
>
> Two — AI-Assisted Code Quality Rate: of code written with AI assistance, what percentage passes all quality gates without rework. Target: above 85%. This ensures AI improves productivity without reducing quality.
>
> Three — Business Case Realization: for AI-powered product features, did we deliver the predicted business outcome? For the conversational AI use case — did care managers spend less time per patient? Target: 80% of AI features meet their stated outcome within 2 quarters."

---

### Q7: Build vs Buy for LLMs?

> "My clear answer: buy the model, build the orchestration and domain layer.
>
> Building an LLM requires massive training data, GPU infrastructure, and ML expertise that very few organizations have. It does not make sense to build unless LLM capability is your core product.
>
> What I build: the orchestration layer (LangChain/LangGraph), the domain-specific tool integrations (OpenSearch connector, patient data retriever), the guardrails and output validation, and the prompt engineering that makes the model useful for our specific use case.
>
> On cost: every organization needs to set a usage budget per engineer. Oracle caps at $100/month per engineer. Beyond that, you need to optimize token usage — caching common queries, using smaller models where precision is not critical, batching where latency is not critical."

---

## SECTION B — COMPANY-BY-COMPANY FEEDBACK SUMMARY

*(Full detail in Company_by_Company_Feedback.md — this is the quick-reference version)*

---

### Availity Round 1 — Seshagiri (Solution Architect) | Grade: 6.5/10

**What worked:** Monolith vs microservices trade-off with real example, Outbox pattern, Splunk + New Relic observability specifics.

**What hurt you:**
- Introduction ran 5+ minutes — he said "you are explaining too much, it's not necessary"
- Payment service incident question was a management question — you answered with Saga and retry patterns
- AI/MCP answer was inconsistent — he caught immediately that "MCP does not insert data into your database"
- Described an MVP that had not started as if it was active development

**Fix going forward:** 90-second introduction maximum. Management questions → management answers first. GenAI story = clean, honest, proposal stage clearly stated.

---

### Availity Round 2 — Ananth (Director of Engineering) | Grade: 6/10

**What worked:** HBase incident story, CAPA framework, CrowdStrike conflict story.

**What hurt you:**
- 10x scaling question: he redirected you 3 times from infrastructure to team/quality/ops — you kept returning to technology
- Logging adequacy question: could not give a specific measurement method for legacy systems
- Said "give me time to think" when asked for executive-level KPIs
- Three executive KPIs were weak and you admitted you had not thought about this before

**Fix going forward:** 10x scaling = lead with team structure and quality governance. Logging = transaction sampling method. KPIs = MTTD, MTTR, Change Failure Rate — memorize these.

---

### Deloitte Round 1 — Santosh (Engineering Leader, L6) | Grade: 7/10

**What worked:** SLA vs SLO distinction was sharp, incident management maturity, hands-on technical involvement was credible, honest about GenAI knowledge gaps, Santosh called you for in-person.

**What hurt you:**
- Claimed "40% accuracy improvement" that was the data science team's work — he called it out
- RAG explanation was inconsistent across the conversation
- Could not give executive KPIs immediately — asked for thinking time

**Fix going forward:** When discussing ML outcomes, say "the data science team achieved X — my team's role was integration, coordination, and delivery." RAG answer = clean and consistent (see File 4).

---

### Deloitte Round 2 — Venkat + Santosh | Grade: 6.5/10

**What worked:** HIPAA data residency clarification was confident and correct, bronze-silver-gold data architecture was strong, three KPIs for AI adoption showed strategic thinking.

**What hurt you:**
- Vibe coding guardrails question: said "honestly I do not have the answer" — for an AI-focused L6 role this was the biggest gap
- 30-60-90 plan: generic listening/one-on-ones — did not address the specific silo problem described
- Could not advise concretely how an architect should use GenAI tools to get 30% efficiency

**Fix going forward:** Guardrails answer is now in Section A Q5. 30-60-90 = address the specific problem (silos) structurally — cross-functional squads, not just listening tours.

---

### Wells Fargo Round 1 — Gurmeet | Grade: 7/10 content, significant tactical errors

**What worked:** Concise product description, AI tool explanation, team structure detail.

**What hurt you — the biggest tactical errors of all interviews:**
- Volunteered layoff status unprompted: "I want to be humble — Oracle gave layoffs and I am one of them." He did not ask. Never do this.
- Gave CTC (66 lakhs) in a screening call. This anchors negotiation before you know the budget.

**Fix going forward:** If asked why leaving = "Oracle went through organizational changes that affected my team. I am using this as an opportunity to find a role where I can take the next step." If asked CTC = "I am flexible and happy to discuss based on the total compensation package."

---

### Cubic (Both Files) | Grade: 6/10

**What worked:** Day-to-day description was structured, CI/CD pipeline detail was specific and credible, blue-green vs canary was correct, OWASP awareness was good.

**What hurt you:**
- Wrong sharding definition: said "sharding means how many shards within a partition" — this is incorrect
- "I am not able to recollect" for Lambda use cases — should know 3 examples cold
- Most damaging moment: "I laid off in Oracle, so I need a job — I don't know either I can tell or put some story on that." This one sentence ends a candidacy if noted.

**Fix going forward:** Sharding = distributing partitions across multiple physical DB instances. Lambda examples: event-driven triggers, glue code between pipeline stages, SLA monitoring alerts. NEVER say you might "put a story" on anything in an interview.

---

## SECTION C — MASTER CHECKLIST

### Things to NEVER Do Again

| Mistake | Context | Fix |
|---------|---------|-----|
| Volunteer layoff | Cubic, Wells Fargo | Only answer if directly asked. Use: "Oracle went through organizational changes affecting my team." |
| Say "I can tell or put a story" | Cubic | Never. Destroys credibility in one sentence. |
| Give CTC in screening | Wells Fargo | "Happy to discuss based on the full package." |
| Answer management question with technical solution | Availity R1, R2 | Lead with management response. Technical is the last third. |
| Say "give me time to think" for standard KPIs | Deloitte R1 | MTTD, MTTR, Change Failure Rate — memorize these. |
| Over-explain platform architecture in introduction | Every interview | 90-second introduction. Stop. Let them ask. |
| Claim ML accuracy improvement your team did not own | Deloitte R1 | "Data science team achieved X. My role was integration and delivery." |
| Inconsistent RAG explanation | Deloitte R1+R2 | Clean answer: Structured = direct query. Unstructured = RAG. Multi-step = LangGraph. |
| Wrong sharding definition | Cubic | Partitioning = within one DB. Sharding = across multiple DB instances. |
| "I am not able to recollect" for basic concepts | Cubic | Lambda: event-driven, short-lived, glue code. Know 3 examples cold. |
| Introduction longer than 90 seconds | All interviews | Timer yourself. Stop at 90 seconds. |

---

### Pre-Interview Discipline Checklist

**Day before:**
```
✅ Review your numbers: $9M saving, 600 microservices, 180 clients, 14-person team
✅ Practice 90-second introduction out loud (timed)
✅ Practice leadership failure story — both 60-second and full versions
✅ Review company background — what product/domain does this company work in?
✅ Prepare 2 questions to ask the interviewer
```

**Interview day:**
```
✅ Management question → management answer first, always
✅ Check in every 3 minutes: "Is this level of detail useful, or should I focus on a different aspect?"
✅ If asked why leaving → "organizational changes" — never mention layoff
✅ If asked CTC → "happy to discuss based on full package"
✅ If you do not know something → "I do not have experience with that specific tool, but the principle I would apply is..." — never "I am not able to recollect"
✅ Numbers ready: 9M, 600, 180, 14, 40%, 60%, 47 minutes, 6 months
```

---

### Memory Hooks — All Files Combined

| Category | Memory Hook |
|---------|------------|
| Introduction | Present → Experience → Achievement → Focus → Vision |
| System design opener | Goals → Metrics → Constraints → NFRs → Levers → Options → Architecture |
| Incident management | Detect → Blast Radius → Mitigate → Communicate → RCA → CAPA |
| Backpressure | Rate Limit → Circuit Breaker → Queue → Reactive → Bulkhead |
| Observability | Logs + Metrics + Traces + Events |
| CI/CD | Quality → Security → Build → Deploy → Govern |
| People management | Clarity → Track → Engage → Intervene |
| Low performers | Identify → Diagnose → Improve → Decide |
| Delivery | Plan → Break → Execute → Risk → Deliver |
| KPIs | Business → Engineering → Reliability |
| Executive KPIs | MTTD + MTTR + Change Failure Rate |
| Stakeholder conflict | Shared Context → Transparent Capacity → Facilitate |
| 6 vs 14 weeks | Validate → Three Options → Ask Why → Never False Commit |
| Leadership failure | Ecosystem Readiness not just Engineering Readiness |
| Scaling 10x | Diagnose → Quick Wins → Horizontal → Data → Ops → Team → Incremental |
| GenAI architecture | Access → Orchestrate → Retrieve → LLM → Guard → Observe |
| High availability | No SPOF → Redundancy → Failover → Multi-Region |
| Production stability | Monitor → Prevent → Learn |

---

### Numbers to Have Ready Cold

| Number | Context |
|--------|---------|
| $9M | Annual savings from OpenShift to Kubernetes migration |
| $24M → $16M | Infrastructure cost before and after migration (Optum) |
| 600 | Microservices migrated (rationalized from) |
| 200 | APIs after rationalization |
| 180 | Care Management clients |
| 120 | Readmission Prevention clients |
| 14 | Team size at Cerner |
| 500M | Patient records processed annually |
| 5M | Daily record processing volume |
| 40% | Velocity improvement after context-switch reduction |
| 60% | Incident rate reduction from legacy integration layer |
| 47 minutes | MTTR for Kafka replay incident |
| 15-20% | Velocity improvement from Oracle Code Assist over 15 months |
| 90% | Code coverage minimum threshold |
| 20% | Tech debt sprint allocation |
| 80% | PI objective completion rate target |
| 85% | Delivery predictability target |
| 5% | Maximum escaped defect rate |
| 30 minutes | P1 MTTR target |
| 5 minutes | P1 MTTD target |

---

## FILE STRUCTURE — YOUR 6-FILE SYSTEM

| File | Contents | When to Review |
|------|---------|---------------|
| **01_Introduction_Career.md** | Self-intro, career walk-through, day-to-day, why leaving | Before every interview |
| **02_People_Behavioral_Stakeholder.md** | Team management, behavioral STAR stories, stakeholder management | Before every interview |
| **03_Delivery_Execution_KPIs.md** | Delivery frameworks, SAFe, tech debt, metrics | Before roles emphasizing delivery/agile |
| **04_SystemDesign_Architecture.md** | Design framework, patterns, distributed systems | Before technical rounds |
| **05_Production_Cloud_DevSecOps.md** | Incidents, observability, AWS/Azure, CI/CD, security | Before technical or SRE-focused rounds |
| **06_GenAI_Feedback_Checklist.md** | GenAI architecture, company feedback, master checklist | Night before every interview |

---

*File 6 of 6 — GenAI, Company Feedback & Master Checklist*
