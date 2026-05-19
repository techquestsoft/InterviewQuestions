# FILE 7 OF 8 — GENAI, AGENTIC AI, AI ADOPTION & PRODUCTIVITY

> **Rule 1:** Be honest about what is in production vs design. Interviewers catch inflated claims immediately.
> **Rule 2:** Your GenAI architecture diagram is your strongest asset — refer to it consistently.
> **Rule 3:** RAG vs direct query has **ONE clean answer**. Don't contradict yourself across the conversation.

**Structure of every answer:** Memory Hook → Core Answer (framework + reasoning) → Example or Discipline Rule (specific scene, anti-pattern to avoid, or honesty framing).

---

## CROSS-FILE INDEX

This file owns: GenAI architecture, RAG patterns, conversational AI, prompt engineering, hallucination handling, GenAI guardrails, AI productivity ROI, agent-driven workflows for engineering automation.

- AI productivity KPIs (cost-per-feature) → also referenced in File 03
- People-side AI adoption → File 02
- Reliability patterns for LLM apps → File 06 (resilience, observability)

---

# SECTION A — GENAI USAGE & STRATEGY

---

## Q1: How are you using GenAI currently?

**Memory Hook:** Productivity Layer + Product Layer (POC honestly)

> **Discipline Rule**
>
> **This honesty point is what cost ground at Availity** (Sheshagiri caught the inconsistency between "MVP" and "not started"). **Always say POC or proposal stage.** Never say *"we are doing the MVP"* if you have not started.

> **Core Answer**
>
> Two levels.
>
> **For engineering productivity** — **Oracle Code Assist** (Claude underlying LLM) installed for every engineer. **Structured prompting templates, not free-form queries.** Oracle's dashboard tracks weekly usage per engineer. After ~15 months: **~20% productivity improvement at team level.** Individually, I track story-point completion before and after — engineers using the tool consistently take on more complex stories over time.
>
> **For product use case** — I have proposed and designed a **conversational AI layer for our Care Management front end**. Today, a care manager searches for one patient at a time to see readmission risk. The use case I am designing would let them ask in natural language: *"Show me the top 10 patients with highest readmission risk this week."* The system translates that to an OpenSearch query, retrieves results, and returns a formatted response.
>
> **This is currently at POC stage with HDI product leadership. I am being transparent about that — it is not in production yet.**

---

## Q2: GenAI architecture — end-to-end

**Memory Hook:** Access → Orchestrate → Retrieve → LLM → Guard → Observe

> **Core Answer — Architecture**
>
> ```
> Care Manager (User)
>     │ Natural language query
>     ▼
> Chat UI (React + WebSocket / REST)
>     Captures query, renders structured results
>     │
>     ▼
> API Gateway / BFF (Spring Boot / FastAPI)
>     Auth, rate limiting, role validation
>     │
>     ▼
> Conversational AI Service (Python + LangChain / OpenAI SDK)
>     Orchestrates the flow, decision logic
>     │
>     ├──► Memory Store (Redis)
>     │       Conversation context (multi-turn)
>     │       Cached patient data for follow-up queries
>     │
>     ├──► Intent & Planner (LLM + Function Calling)
>     │       Understands query intent
>     │       Selects which tool to call
>     │
>     └──► Tool Layer (OpenAI Functions / LangChain Tools)
>             │
>             ├──► OpenSearch Tool
>             │       OpenSearch / REST API
>             │       Fetch patient records by risk score
>             │       Filter, sort, aggregate
>             │
>             └──► RAG Tool
>                     Vector DB (FAISS / Pinecone)
>                     Retrieve clinical guidelines, drug interactions
>                     Provide explanations
>     │
>     ▼
> Data Stores
>     OpenSearch Index ← Patient Risk Data (structured)
>     Vector Database  ← Clinical Knowledge Base (unstructured)
>     │
>     ▼
> Response Generator (GPT-4 / Azure OpenAI)
>     Generate natural language response from retrieved data
>     │
>     ▼
> UI Renderer (React Components)
>     Tables, charts, risk explanations
> ```
>
> **Six layers explained**
>
> | Layer | What | Tools |
> |-------|------|-------|
> | **Access** | API auth, rate limiting | Spring Boot / FastAPI |
> | **Orchestrate** | Prompt management, workflow | LangChain / LangGraph |
> | **Retrieve** | Data fetching from sources | OpenSearch, Vector DB |
> | **LLM** | Generation | GPT-4 / Azure OpenAI |
> | **Guard** | Security, validation, content filters | Custom + LLM-based validation |
> | **Observe** | Monitoring, logging, cost tracking | New Relic + custom dashboards |

---

## Q3: How do you design a GenAI solution?

**Memory Hook:** Input → Process → Output, Layered Architecture

> **Core Answer**
>
> Layered architecture with clear separation of concerns.
>
> **Input layer** — clean and validate user input. **Strip PII before it reaches the LLM** where possible.
>
> **Processing layer** — orchestration, intent recognition, tool selection. This is where LangChain or LangGraph lives.
>
> **Output layer** — response generation, validation, formatting for UI.
>
> **Combine RAG + direct query based on data type** — structured data goes through direct query (OpenSearch native vector search), unstructured documents go through RAG (Vector DB).
>
> Multi-tenant governance, cost optimization through caching and model selection, and observability for cost-per-request and adoption.

---

# SECTION B — RAG, RETRIEVAL & DECISION MAKING

---

## Q4: RAG vs Direct Query — when to choose each

**Memory Hook:** Structured = Direct | Unstructured = RAG | Multi-step = LangGraph

> **Discipline Rule**
>
> **This is the question that tripped you up at Deloitte across both rounds.** **Memorize this clean answer.** Saying *"RAG not needed... actually maybe needed... LangGraph might handle it"* is what cost grade. **The clean rule: Structured = direct query. Unstructured = RAG. Multi-step = LangGraph.**

> **Core Answer**
>
> | Scenario | Approach | Reasoning |
> |----------|---------|-----------|
> | "Top 10 readmission patients" | **Direct OpenSearch query** | Structured data, deterministic, low latency |
> | "What medications reduce readmission for CHF patients?" | **RAG with Vector DB** | Unstructured clinical docs, semantic search needed |
> | "Show top 10 patients, then filter by hospital, then show their medications" | **LangGraph orchestration** | Multi-step, output of one step feeds next |
>
> **Spoken answer**
>
> For our readmission use case, OpenSearch's **native K-NN vector search** is sufficient for the initial retrieval — I do not need a separate RAG pipeline or external vector database for structured patient queries.
>
> RAG becomes relevant when we want the LLM to answer questions from **unstructured clinical notes or guidelines stored as documents**. That is Phase 2.
>
> For **multi-step conversations**, LangGraph orchestrates context across turns and chains queries together.

---

## Q5: Chunking & Embeddings

**Memory Hook:** Chunk → Embed → Index → Retrieve

> **Core Answer**
>
> Break large documents into smaller, semantically meaningful chunks. Convert each chunk into a vector embedding. Store embeddings in a vector database with metadata. At query time, embed the user's query and retrieve the **top-k most similar chunks** via vector similarity search.
>
> **Chunking strategy.** **Semantic chunking with overlap** — typically **500 to 1000 tokens per chunk with 100–200 token overlap** so context spans chunk boundaries. **Pure size-based chunking breaks sentence and paragraph boundaries — that hurts retrieval quality.**
>
> **Embedding model choice.** OpenAI's `text-embedding-3` or `sentence-transformers` for cost efficiency. **Trade-off between embedding quality and cost.**
>
> **Retrieval.** Vector similarity (cosine similarity) for the top-k. **Re-ranking with a smaller model for better precision** when needed.

---

## Q6: Hallucination handling

**Memory Hook:** Ground → Constrain → Validate → Human-in-the-Loop

> **Core Answer**
>
> Four mechanisms.
>
> **Grounding via retrieval.** The LLM answers **only from retrieved data, not general knowledge**. Prompt explicitly says *"answer only using the following patient records"* — not *"use your knowledge."*
>
> **Prompt constraints.** System prompt defines scope, format, and what the model must not do. For clinical: *"Do not recommend specific medications. Do not make diagnostic statements. Only summarize what is in the retrieved records."*
>
> **Output validation.** Validate response structure before displaying. If response format does not match expected schema, reject and return graceful error.
>
> **Human-in-the-loop.** For high-stakes decisions like clinical recommendations, **LLM response is presented as context for the care manager's judgment — not as a directive.** The care manager decides. The AI informs.

---

# SECTION C — GUARDRAILS, SECURITY, COST

---

## Q7: How do you build guardrails for GenAI in an engineering team?

**Memory Hook:** Approved Tools → Structured Prompting → CI Gates → Measurement

> **Discipline Rule**
>
> **This was the biggest gap in the Deloitte 2nd round.** You said *"honestly I do not have the answer."* Now you do.

> **Core Answer**
>
> Three levels of guardrails plus measurement.
>
> **Approved tooling.** Company-approved AI tools only — **no personal ChatGPT, no external tools that might expose patient data**. Oracle Code Assist stays within Oracle's cloud boundary.
>
> **Structured prompting standards.** Prompting templates defined for common engineering tasks — feature scaffolding, test generation, code review, documentation. **Engineers do not free-form prompt.** Templates embed our coding standards, security patterns, exception handling.
>
> **CI pipeline gates.** **AI-generated code is not exempt from code review or coverage requirements.** I would add a step that flags large blocks of code matching common AI patterns — high cyclomatic complexity with no exception handling, test files with only happy-path cases. Mandatory senior review.
>
> **Measurement.** Track AI-assisted code review pass rate on first submission vs rework rate. **If AI-assisted code escapes more defects than manually coded features, the guardrails are not working — tighten them.**
>
> **Honest acknowledgment:** this space is evolving and perfect enforcement does not exist yet. What I commit to: **guardrails explicit, automated where possible, iteratively tightened based on data.**

---

## Q8: How do you advise an architect to use GenAI for 30% efficiency gain?

**Memory Hook:** First Draft → Critique → Devil's Advocate → Documentation

> **Discipline Rule**
>
> **This was Venkat's Deloitte question — your answer was too vague.** Specific workflow below.

> **Core Answer**
>
> I give the architect a **specific workflow, not just permission**.
>
> **For new system design.** Start with GenAI to generate the **first-draft architecture** from a structured prompt — system requirements, scale, NFRs, technology constraints. The architect's job is **not starting from a blank page but critiquing the GenAI output**: what did it get right, what did it miss, what would not work in our context. **Critique is faster than creation.**
>
> **For architecture reviews.** Use GenAI to generate a **devil's-advocate critique** — *"what are the failure modes of this design? What breaks at 10x scale?"* Surfaces blind spots faster than waiting for peer review cycle.
>
> **For documentation.** GenAI generates the architecture document from decisions already made. Architect reviews and corrects. **Documentation that took two days takes four hours.**
>
> **Measurement.** Time from requirements handoff to architecture sign-off, compared to baseline. **Track as a trend, not a point measurement** — architect improves prompting over time.

---

## Q9: GenAI Security

**Memory Hook:** Boundary → Auth → Audit → No PII Leakage → Tenant Isolation

> **Core Answer**
>
> Five practices.
>
> **Cloud boundary enforcement.** LLM hosted within our cloud tenant — Oracle's case, within Oracle Cloud — **data does not cross to public OpenAI APIs.**
>
> **Role-based access.** Care managers query only their authorized patient population. **Auth checked at API Gateway, enforced again at service layer.**
>
> **Audit logging.** Every query logged with user, timestamp, intent, and tools called. Audit trail for compliance review.
>
> **No PHI in LLM prompt directly.** We pass **de-identified query parameters and IDs**. The LLM never sees PHI in the natural-language prompt — only IDs that are resolved against authorized records.
>
> **Data isolation.** Multi-tenant design — **one tenant's data is never accessible across tenant boundary.** Vector DB indexed per tenant where applicable.

---

## Q10: Cost vs Performance trade-off

**Memory Hook:** Cap Per User → Cache → Smaller Models Where Possible

> **Core Answer**
>
> Every organization needs a usage budget. **Oracle caps at $100/month per engineer for Code Assist.** Beyond that, optimize:
>
> - **Caching common queries** — same intent across users hits cache, not the LLM
> - **Smaller models where precision is not critical** — GPT-4 for complex reasoning, smaller models for routine tasks
> - **Batching where latency is not critical** — async processing for non-real-time workflows
> - **Token optimization** — concise prompts, structured output formats, no unnecessary context
> - **Model abstraction layer** — easy to swap models as cost/capability changes

---

## Q11: Scaling GenAI

**Memory Hook:** Rate Limit → Async → Smaller Models → Cost Controls → Cache

> **Core Answer**
>
> Standard scaling principles plus AI-specific concerns.
>
> **Rate limiting.** Per user, per tenant, per LLM provider. **Prevents one tenant from exhausting shared LLM quota.**
>
> **Async processing.** For non-real-time use cases, queue requests, process in batch. Reduces peak load.
>
> **Model optimization.** Use smaller, faster models for routine tasks. Reserve large models for complex reasoning.
>
> **Cost control.** Per-user budgets, per-tenant quotas. **Alerts when usage exceeds threshold.**
>
> **Caching.** Common queries cached. Significant cost saving for repeated patterns.

---

# SECTION D — BUILD vs BUY, MEASUREMENT, ROI

---

## Q12: Build vs Buy for LLMs?

**Memory Hook:** Buy Model, Build Orchestration + Domain Layer

> **Core Answer**
>
> Clear answer: **buy the model, build the orchestration and domain layer.**
>
> **Building an LLM** requires massive training data, GPU infrastructure, and ML expertise that very few organizations have. **Does not make sense unless LLM capability is your core product.**
>
> **What I build:**
> - Orchestration layer (LangChain / LangGraph)
> - Domain-specific tool integrations (OpenSearch connector, patient data retriever)
> - Guardrails and output validation
> - Prompt engineering for our specific use case
>
> **Decision criteria:** cost vs speed. **Buy is faster to value. Build is only justified if you have core differentiation in the model itself.**

---

## Q13: Prompt Engineering — frameworks and templates

**Memory Hook:** Role → Task → Context → Constraints → Format

> **Core Answer**
>
> Structured prompting using a consistent framework.
>
> **Role.** *"You are a clinical decision support assistant."*
>
> **Task.** *"Summarize the patient's readmission risk factors."*
>
> **Context.** *"The patient's records are below: [data]."*
>
> **Constraints.** *"Do not recommend specific medications. Do not make diagnostic statements. Use only the data provided."*
>
> **Format.** *"Respond in 3 bullet points, max 50 words total."*
>
> **Discipline rule**
>
> **Templates are versioned in Git. Changes go through review like code. Prompt regression tests run before deployment** — same input should produce semantically same output across versions.

---

## Q14: How do you measure productivity improvement from AI tools?

**Memory Hook:** Velocity is a SIGNAL, Cost-per-Feature is the METRIC

> **Discipline Rule**
>
> **This is Ananth's challenge at Availity** — *"velocity is not business value."* **Your answer needed to lead with cost-per-feature.**

> **Core Answer**
>
> Two levels.
>
> **Team level (signal):** Sprint velocity is a starting point — tells me if engineers are completing more work per sprint. **But it does not tell me if that work has business value. It is a signal, not a verdict.**
>
> **Business level (real metric):** **Cost per feature delivery.** If implementing a feature of known scope and complexity previously cost X engineering hours at a known rate, and with AI assistance it costs 0.8X, that is **20% reduction in cost per feature**. Applied across a quarter's roadmap, that translates directly to dollars saved or additional features delivered within the same budget.
>
> **Second business metric:** **Time to market** — days from feature sign-off to production deployment. If AI compresses design, coding, and review cycles, measurable in days. Direct revenue or NPS implications.
>
> **Track both quarterly as trend lines, not point-in-time.** Productivity gain compounds as engineers improve their prompting.

---

## Q15: Three KPIs for AI adoption (executive view)

**Memory Hook:** Velocity + Quality Rate + Business Realization

> **Core Answer**
>
> Three KPIs that tell an executive whether AI investment is paying off.
>
> **One — Feature Delivery Velocity.** Time from feature sign-off to production deployment, vs pre-AI baseline. **Target: 20% reduction in 6 months.**
>
> **Two — AI-Assisted Code Quality Rate.** Of code written with AI assistance, what percentage passes all quality gates without rework. **Target: above 85%.** Ensures AI improves productivity without reducing quality.
>
> **Three — Business Case Realization.** For AI-powered features, did we deliver the predicted business outcome? **Target: 80% of cases meet stated outcome within 2 quarters.**
>
> **Together:** are we faster, faster without quality loss, and does speed actually produce business value.

---

## Q16: How do you build a GenAI engineering team?

**Memory Hook:** Platform → Product → Governance → Upskill

> **Core Answer**
>
> Four-component model.
>
> **Platform Team** owns core GenAI capabilities — LLM orchestration, model abstraction, prompt template library, guardrails, observability. **Reusable across product teams.**
>
> **Product Teams** consume the platform and implement use cases. **They focus on the user-facing application, not the LLM plumbing.**
>
> **Governance** — security and compliance. PHI handling, audit logging, model approval list, cost controls.
>
> **Upskilling** — train existing engineers on prompt engineering, agent design, RAG patterns. Hire selectively for ML/AI specialists where needed. **Most of the work is software engineering with AI integration, not ML research.**

---

# SECTION E — AGENTIC AI, RESPONSIBLE AI & LLM RELIABILITY (JPMC PREFERRED QUALS)

> *The JPMC JD explicitly lists three preferred qualifications around AI:*
> *1. Experience integrating AI/LLM capabilities into applications*
> *2. Ability to drive adoption of agent-style tools/workflows for automation and orchestration*
> *3. Familiarity with reliability patterns for LLM applications and responsible AI fundamentals*
>
> *Sections A–D cover (1). This section covers (2) and (3) explicitly.*

---

## Q17: How would you drive adoption of agent-style tools and workflows for automation?

**Memory Hook:** Identify Toil → Pilot → Measure → Scale → Govern

> **Core Answer**
>
> Five steps. Same playbook used at Cerner for AI-assisted engineering — applied to agentic workflows.
>
> **Identify high-toil workflows.** Where engineers spend time on **repetitive, structured work** — PR review summarization, log triage during incidents, runbook execution, ticket classification, on-call first-response, code refactoring. **Agents excel here, not at open-ended creative work.**
>
> **Pilot with one team.** Pick a willing team and one workflow with measurable success criteria — say, **reduce time-to-triage by 30% on Sev3 incidents**. Don't boil the ocean.
>
> **Measure honestly.** Time saved per task, error rate compared to baseline, engineer satisfaction. **Publish the numbers — including the misses.**
>
> **Scale with templates and guardrails.** Reusable agent patterns, approved tools, action-level audit logs. **Engineers get an opinionated platform, not "go build your own agent."**
>
> **Govern from day one.** Agents have access; **access has consequences.** RBAC on agent tool calls, blast-radius limits (read-only by default, write actions require explicit approval), **human-in-the-loop for irreversible operations**, full audit trail.
>
> **Honest framing**
>
> I have led adoption of AI-assisted engineering practices at Cerner that delivered roughly **20% productivity improvement**. Full agentic workflows — where the agent autonomously completes multi-step tasks — are **still earlier-stage industry-wide**. I'd bring the adoption playbook and the governance discipline; **I would not claim to have shipped a fully autonomous agent in production.**

---

## Q18: What reliability patterns do you apply to LLM applications?

**Memory Hook:** Timeout → Retry → Fallback → Cache → Cost Control

> **Core Answer**
>
> Five patterns. Same reliability discipline as any other distributed system, plus LLM-specific concerns.
>
> **Timeouts and retries.** LLM APIs are slow and occasionally fail. **Aggressive timeouts (5–10 seconds for synchronous flows)**, exponential backoff, **jittered retries to avoid thundering herd** on the provider.
>
> **Fallback strategy.** Primary model unavailable? Failover to a secondary provider or a smaller model. For non-critical paths, **degrade gracefully** — return a deterministic answer or a "try again" with a fast path. **Hard dependency on one provider is a single point of failure.**
>
> **Caching aggressively.** **Semantic cache for similar queries** (vector similarity on prompts), exact-match cache for repeated identical prompts. Cuts cost and latency dramatically — **often 30–60% hit rate on production workloads.**
>
> **Rate limiting and quotas.** Per-user, per-tenant, per-feature. **Prevents abuse and runaway cost.** Especially important when end-users can compose prompts that trigger expensive model calls.
>
> **Cost controls as first-class.** Token budgets per request, smaller models for simpler tasks, prompt optimization (shorter system prompts, response length caps). **LLM costs surprise teams — I've seen monthly bills 5x what was forecast because nobody set caps.**
>
> **Plus standard reliability discipline (File 06):** structured logging with correlation IDs, p95/p99 latency tracking per model, error rate per provider, dashboards distinguishing *"our error"* from *"provider error."*

---

## Q19: What does responsible AI mean to you in a banking context?

**Memory Hook:** Bias → Explainability → Human Review → Audit → Kill Switch

> **Core Answer**
>
> Five fundamentals. **In banking, responsible AI is a regulatory requirement, not an ethics nice-to-have.**
>
> **Bias testing.** Models can encode bias from training data. For customer-impacting decisions — credit, account approval, fraud scoring — **bias testing across demographic dimensions before launch and continuously in production**. Track approval rates and outcomes by protected attribute classes.
>
> **Explainability.** Customer-impacting AI decisions need a reason that can be communicated to the customer and to regulators. **For credit decisions, fair-lending laws require this.** SHAP values, feature importance, or rule-based fallbacks for highest-stakes decisions.
>
> **Human review for high-stakes outputs.** **AI suggests; humans decide on irreversible or high-impact actions.** Account closure, large transaction blocks, escalations to fraud — humans in the loop.
>
> **Audit logging.** Every prompt, every response, every tool call logged with **who, what, when, model version**. Required for regulator inquiries and for retroactive review when something goes wrong.
>
> **Kill switches and rollback.** **Ability to disable a model or revert to deterministic logic instantly** when behavior drifts or compliance issues surface. Feature-flag the AI path so it can be turned off without redeploying.
>
> **Honest framing**
>
> Responsible AI is an **organizational discipline, not a single engineer's responsibility**. As an engineering manager, my role is to ensure the right governance reviews are in place — model approval boards, security review, legal review — and that **my engineers know they can stop a launch if something feels wrong.**

---

## Q20: How would you integrate AI into the BBAO account origination journey specifically?

**Memory Hook:** Pain Point → Solution → Architecture → Measure

> **Core Answer**
>
> I'd start where customer pain is concrete and measurable.
>
> **Pain point.** Customers abandon onboarding because of **complex forms, document-upload friction, slow KYC verification, and unclear status during processing.**
>
> **Three high-value AI plays**
>
> **1. Smart document understanding** — vision + LLM extracts data from uploaded IDs and supporting documents, pre-fills forms, reduces typing. **Confidence threshold; below threshold routes to human review with summarized context.**
>
> **2. Conversational assistant** — answers customer questions in real time during the journey. RAG over policies, FAQ, regulatory disclosures. **Scoped strictly — never gives investment or eligibility advice.** Hand-off to human agent when out of scope or low confidence.
>
> **3. Intelligent agent triage** — classifies edge cases for human reviewers and surfaces a structured summary so the agent isn't reading through every uploaded document. Reduces handle time per escalation.
>
> **Architecture**
>
> Document processor → entity extraction → form pre-fill API → **guardrails (PII redaction, accuracy threshold, content policy)** → human fallback when confidence is low. Conversational layer is a separate service with its own RAG store and model gateway.
>
> **Measure**
>
> Onboarding completion rate, time-to-complete, customer satisfaction, agent handle time on escalations, accuracy of auto-extracted data. **Bias and fairness metrics across customer demographics.**
>
> **Honest framing**
>
> I'd lead the engineering side and partner closely with **Risk, Compliance, and Legal** on responsible AI requirements. **I would not propose AI for the core credit or eligibility decision itself** — that's a regulated decision space where human-in-the-loop and explainability requirements are highest, and where the bank likely has existing model governance frameworks I'd plug into rather than reinvent.

---

# QUICK REFERENCE — MEMORY HOOKS

| # | Topic | Memory Hook |
|---|---|---|
| Q1 | Current GenAI usage | Productivity + Product (POC honestly) |
| Q2 | GenAI architecture | Access → Orchestrate → Retrieve → LLM → Guard → Observe |
| Q3 | Design a GenAI solution | Input → Process → Output, Layered |
| Q4 | RAG vs Direct Query | Structured = Direct / Unstructured = RAG / Multi-step = LangGraph |
| Q5 | Chunking & Embeddings | Chunk → Embed → Index → Retrieve |
| Q6 | Hallucination handling | Ground → Constrain → Validate → Human-in-the-Loop |
| Q7 | Engineering guardrails | Approved Tools → Structured Prompts → CI Gates → Measurement |
| Q8 | Architect 30% efficiency | First Draft → Critique → Devil's Advocate → Documentation |
| Q9 | GenAI security | Boundary → Auth → Audit → No PII → Tenant Isolation |
| Q10 | Cost vs performance | Cap Per User → Cache → Smaller Models |
| Q11 | Scaling GenAI | Rate Limit → Async → Smaller Models → Cost Controls → Cache |
| Q12 | Build vs Buy | Buy Model, Build Orchestration + Domain |
| Q13 | Prompt engineering | Role → Task → Context → Constraints → Format |
| Q14 | Productivity measurement | Velocity = Signal, Cost-per-Feature = Metric |
| Q15 | AI executive KPIs | Velocity + Quality Rate + Business Realization |
| Q16 | Building a GenAI team | Platform → Product → Governance → Upskill |
| Q17 | Agent adoption | Identify Toil → Pilot → Measure → Scale → Govern |
| Q18 | LLM reliability patterns | Timeout → Retry → Fallback → Cache → Cost Control |
| Q19 | Responsible AI (banking) | Bias → Explainability → Human Review → Audit → Kill Switch |
| Q20 | BBAO origination AI | Pain Point → Solution → Architecture → Measure |

---

# APPENDIX A — AI DECISION MATRIX

| Question | Answer |
|---------|--------|
| Structured query, deterministic? | **Direct query** (OpenSearch native) |
| Unstructured documents? | **RAG with Vector DB** |
| Multi-step reasoning? | **LangGraph orchestration** |
| Real-time low latency? | Direct query, cache aggressively |
| High accuracy critical? | Larger model + grounding + human-in-loop |
| Cost-sensitive routine task? | Smaller model, cache common queries |
| PHI / regulated data? | Stay within cloud tenant boundary, no public APIs |
| Multi-tenant? | Per-tenant rate limits, isolated vector indices |

---

# APPENDIX B — CONSISTENCY RULES (MEMORIZE)

| Topic | Clean Answer |
|-------|--------------|
| Status of conversational AI use case | **"POC stage — designed and validated, not in production"** |
| RAG decision rule | **Structured = direct query; unstructured = RAG; multi-step = LangGraph** |
| LLM hosting | **Within Oracle Cloud tenant — PHI does not leave the boundary** |
| Productivity measurement | **Velocity = signal; cost-per-feature + time-to-market = real metrics** |
| Build vs buy | **Buy model, build orchestration and domain layer** |
| Guardrails | **Approved tools + structured prompts + CI gates + measurement** |

---

# APPENDIX C — WHAT NEVER TO SAY ABOUT GenAI

| Wrong | Right |
|-------|-------|
| *"We are doing the MVP"* (when not started) | **"This is at POC / proposal stage"** |
| *"MCP inserts data into the database"* | **"MCP provides connectivity. The LLM generates the query, OpenSearch executes it"** |
| *"RAG not needed... actually maybe needed... LangGraph maybe"* | **One clean rule: structured = direct, unstructured = RAG, multi-step = LangGraph** |
| *"Honestly I don't have the answer"* (on guardrails) | **Use the 4-element framework: tools, prompting standards, CI gates, measurement** |
| *"We achieved 40% accuracy improvement"* (claim DS team's work) | **"The data science team achieved ~40% accuracy. My team's role was integration, coordination, delivery"** |

---

# APPENDIX D — KEY NUMBERS FOR GenAI ANSWERS

| Metric / Anchor | Value |
|---|---|
| Oracle Code Assist productivity gain | ~20% over ~15 months |
| Cerner team adoption period | 15 months |
| Oracle Code Assist per-engineer budget | $100/month |
| Cost-per-feature reduction with AI (illustrative) | 20% (0.8X cost) |
| AI-Assisted Code Quality Rate target | >85% pass on first submission |
| Feature delivery velocity target | 20% reduction in 6 months |
| Business case realization target | 80% of cases hit outcome within 2 quarters |
| Semantic cache hit rate on production | 30–60% typical |
| LLM synchronous timeout | 5–10 seconds |
| Chunking size for RAG | 500–1000 tokens with 100–200 token overlap |
| Oncology mapping coverage threshold (cross-ref File 08) | ≥ 95% |
| V3 ML model accuracy improvement | ~40% (DS team's work — credit accurately) |

---

*File 7 of 8 — GenAI, Agentic AI, AI Adoption & Productivity (merged master)*
