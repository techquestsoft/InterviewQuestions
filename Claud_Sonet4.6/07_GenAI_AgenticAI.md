# FILE 7 OF 8 — GENAI, AGENTIC AI, AI ADOPTION & PRODUCTIVITY

> **Rule 1:** Be honest about what is in production vs design. Interviewers catch inflated claims immediately.
> **Rule 2:** Your GenAI architecture diagram is your strongest asset — refer to it consistently.
> **Rule 3:** RAG vs direct query has **ONE clean answer**. Don't contradict yourself across the conversation.

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

**Memory Hook:** Engineering Productivity + Product Use Case (POC Stage)

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
> API Gateway / Backend for Frontend (BFF) (Spring Boot / FastAPI)
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

# SECTION B — RAG, RETRIEVAL & DECISION MAKING

---

## Q4: RAG vs Direct Query — when to choose each

**Memory Hook:** Structured = Direct Query | Unstructured = RAG | Multi-Step = LangGraph

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

**Memory Hook:** Grounding via Retrieval → Prompt Constraints → Output Validation → Human-in-the-Loop

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

**Memory Hook:** Approved Tooling → Structured Prompting Standards → CI Pipeline Gates → Measurement

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
> **CI pipeline gates.** **AI-generated code is not exempt from code review or coverage requirements.** I would enforce CI/CD pipeline quality checks so AI-generated code follows the same standards as manually written code. AI-generated code should still require proper code reviews, test coverage, and security validation.
> 
> I would also add automated checks to identify risky patterns often seen in AI-generated code, such as overly complex logic, missing exception handling, or tests covering only successful scenarios. For high-risk or large AI-generated changes, I would require mandatory senior engineer review before merge.
>
> **Measurement.** Track AI-assisted code review pass rate on first submission vs rework rate. **If AI-assisted code escapes more defects than manually coded features, the guardrails are not working — tighten them.**
>
> **Honest acknowledgment:** this space is evolving and perfect enforcement does not exist yet. What I commit to: **guardrails explicit, automated where possible, iteratively tightened based on data.**

---

## Q8: How do you advise an architect to use GenAI for 30% efficiency gain?

**Memory Hook:** First-Draft Architecture → Critique > Creation → Devil's-Advocate Reviews → Documentation Generation

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

**Memory Hook:** Cloud Boundary → Role-Based Access → Audit Logging → No PHI in Prompt → Data Isolation

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

## Q10: Cost vs Performance — and scaling GenAI

**Memory Hook:** Usage Budget Cap → Cache Common Queries → Smaller Models Where Possible → Batching → Rate Limiting → Token Optimization

> **Core Answer**
>
> Every organisation needs a usage budget. **Oracle caps at $100/month per engineer for Code Assist.** Beyond that, four levers:
>
> **Caching common queries** — same intent across users hits cache, not the LLM. Significant cost saving for repeated patterns.
>
> **Model selection by task** — GPT-4 for complex reasoning, smaller models for routine tasks. Match model cost to task complexity.
>
> **Rate limiting and async processing** — per user, per tenant, per LLM provider to prevent quota exhaustion. For non-real-time use cases, queue requests and process in batch to reduce peak load.
>
> **Token optimization** — concise prompts, structured output formats, no unnecessary context. Cost scales with tokens.
>
> **Model abstraction layer** — easy to swap models as cost/capability changes. Avoids LLM vendor lock-in.

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

**Memory Hook:** Team Velocity = Signal,  Cost-per-Feature + Time-to-Market = Business Metrics

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

**Memory Hook:** Feature Delivery Velocity + AI-Assisted Code Quality Rate + Business Case Realization

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

**Memory Hook:** Platform Team → Product Teams → Governance → Upskilling

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

**Memory Hook:** Identify High-Toil Workflows → Pilot With One Team → Measure Honestly → Scale With Templates → Govern From Day One

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

**Memory Hook:** Timeouts + Retries → Fallback Strategy → Aggressive Caching → Rate Limiting → Cost Controls

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

**Memory Hook:** Bias Testing → Explainability → Human Review → Audit Logging → Kill Switches

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

# QUICK REFERENCE — MEMORY HOOKS

| # | Topic | Memory Hook |
|---|---|---|
| Q1 | Current GenAI usage | Engineering Productivity + Product Use Case (POC Stage Honestly) |
| Q2 | GenAI architecture | Access → Orchestrate → Retrieve → LLM → Guard → Observe |
| Q3 | Design a GenAI solution | Input Layer → Processing Layer → Output Layer (Layered Architecture) |
| Q4 | RAG vs Direct Query | Structured = Direct Query / Unstructured = RAG / Multi-Step = LangGraph |
| Q5 | Chunking & Embeddings | Chunk → Embed → Index → Retrieve |
| Q6 | Hallucination handling | Grounding via Retrieval → Prompt Constraints → Output Validation → Human-in-the-Loop |
| Q7 | Engineering guardrails | Approved Tooling → Structured Prompting Standards → CI Pipeline Gates → Measurement |
| Q8 | Architect 30% efficiency | First-Draft Architecture → Critique > Creation → Devil's-Advocate Reviews → Documentation Generation |
| Q9 | GenAI security | Cloud Boundary → Role-Based Access → Audit Logging → No PHI in Prompt → Data Isolation |
| Q10 | Cost vs performance | Usage Budget Cap → Cache Common Queries → Smaller Models Where Possible → Batching → Token Optimization |
| Q11 | Scaling GenAI | Rate Limiting → Async Processing → Model Optimization → Cost Control → Caching |
| Q12 | Build vs Buy | Buy Model, Build Orchestration + Domain Layer |
| Q13 | Prompt engineering | Role → Task → Context → Constraints → Format |
| Q14 | Productivity measurement | Team Velocity = Signal / Cost-per-Feature + Time-to-Market = Business Metrics |
| Q15 | AI executive KPIs | Feature Delivery Velocity + AI-Assisted Code Quality Rate + Business Case Realization |
| Q16 | Building a GenAI team | Platform Team → Product Teams → Governance → Upskilling |
| Q17 | Agent adoption | Identify High-Toil Workflows → Pilot With One Team → Measure Honestly → Scale With Templates → Govern From Day One |
| Q18 | LLM reliability patterns | Timeouts + Retries → Fallback Strategy → Aggressive Caching → Rate Limiting → Cost Controls |
| Q19 | Responsible AI (banking) | Bias Testing → Explainability → Human Review → Audit Logging → Kill Switches |

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

---

# SECTION F — CORRECTIONS AND ADDITIONS FROM REAL INTERVIEWS

## F1: LLM tool invocation — LLMs CAN invoke tools

**Correction source:** Globalogic Round 2 (Fazeq). You said or implied LLMs cannot perform any action. Wrong.

**Memory Hook:** LLM selects the tool and parameters → host application executes → result back to LLM

> **Correct answer:**
>
> Modern LLMs support function calling and tool use natively. You define tools as JSON schemas — name, description, parameters. When the LLM receives a user query, it reads the tool descriptions and decides which tool to invoke and with what parameters. The host application executes the tool call, gets the result, passes it back to the LLM, which generates the final response.
>
> **The LLM selects the tool and provides parameters. The orchestration layer (LangChain/LangGraph) executes it.** This is the foundation of agentic AI. Never say "LLM cannot invoke tools."

---

## F2: Data containment — lead with infrastructure, not system prompting

**Correction source:** Globalogic Round 2 (Fazeq). You said system prompting contains the data. Fazeq corrected: install the LLM in your own infrastructure.

**Memory Hook:** Infrastructure (strongest) → System Prompting (behavioral) → Contractual (legal)

> **Correct answer — three layers in order of strength:**
>
> **First and strongest — infrastructure containment.** Deploy the LLM within your own cloud boundary. Azure: Azure OpenAI Service with private endpoints — traffic never reaches the public OpenAI API. OCI: Oracle GenAI within the OCI boundary. AWS: Bedrock with VPC endpoints. When the model runs inside your infrastructure, data physically cannot leave. This is the only truly reliable technical containment.
>
> **Second — system prompting and grounding.** Instruct the model to answer only from retrieved context, not general knowledge. This is a behavioral control — the model follows the instruction but it is not a technical guarantee. A model can sometimes deviate.
>
> **Third — contractual controls.** Vendor agreements that your data won't be used for model training. Legal, not technical.
>
> In a HIPAA environment, all three layers are required. Infrastructure containment is non-negotiable — you cannot rely on system prompting alone.

---

## F3: AI Delivery Lifecycle (AI DLC)

**Source:** Globalogic Round 3 prep. Biju explicitly said to prepare this. It extends traditional SDLC with AI-specific phases.

**Memory Hook:** Plan → Data → Model → Integrate → Govern → Operate → Improve

> **Core Answer**
>
> Traditional SDLC covers requirements through deployment. AI DLC extends that with phases specific to model development and governance.
>
> **Plan** — define the AI use case, success metrics, data requirements, build-vs-buy decision. Most teams skip this. That's why AI projects fail to deliver value.
>
> **Data** — discovery, quality assessment, labeling, pipeline setup. In healthcare: de-identification, consent, HIPAA compliance. Data phase is typically 40% of total effort.
>
> **Model** — select or fine-tune the model, validate, measure accuracy against baseline. For most enterprise use cases: select a foundation model and do RAG or fine-tuning, not train from scratch.
>
> **Integrate** — embed the model into the product. API design, tool configuration for agentic flows, UI integration, latency testing, safety testing.
>
> **Govern** — security review, compliance review, bias testing, explainability, audit logging, human-in-the-loop design. In regulated industries this is a gate, not optional.
>
> **Operate** — deploy, monitor model performance, track cost per query, alert on drift or accuracy degradation, manage LLM provider SLAs.
>
> **Improve** — feedback loops from production usage, retraining triggers, prompt optimization, model upgrades. AI systems degrade over time without active maintenance — this is model drift.

---

## F4: Shadow mode — AI validation before go-live

**Source:** Globalogic Round 2 (Fazeq). Use this term when describing AI rollout strategy.

**Memory Hook:** AI runs in parallel → outputs logged internally → nothing affects real users → validate before cutover

> **Correct answer:**
>
> Shadow mode means running the AI model in parallel with the existing system. AI generates outputs, those outputs are logged internally and compared against the existing system's outputs or against ground truth. Nothing reaches real users until accuracy is validated.
>
> This is the only responsible way to introduce clinical AI into a production environment. "Trust but verify" before you flip the switch.
>
> Shadow mode is how we validated the V3 ML model accuracy improvement before replacing V1/V2 as the primary scoring engine.

---

## F5: Embedding model vs generation model — clean distinction

**Correction source:** Wells Fargo Round 2. When asked "what model are you using to embed the documents?" the answer was "Claude Opus 4.3." Claude Opus is a generation model, not an embedding model. These are completely different model types.

**Memory Hook:** Generation model = produces text → Embedding model = produces vectors

> **The distinction:**
>
> **Generation models** (Claude, GPT-4, Gemini) take text in and produce text out. They generate responses, summaries, code, explanations. Claude Opus, Sonnet, Haiku are generation models.
>
> **Embedding models** take text in and produce a fixed-length vector (a list of numbers) that represents the semantic meaning of that text. These vectors are stored in a vector database and compared mathematically at retrieval time. Embedding models do not generate text.
>
> **Common embedding models:**
> - OpenAI: `text-embedding-3-small`, `text-embedding-3-large`
> - Cohere: `embed-english-v3.0`
> - AWS Bedrock: Amazon Titan Embeddings G1
> - HuggingFace open source: `sentence-transformers/all-MiniLM-L6-v2`
>
> **In a RAG pipeline the two are used together but separately:**
> 1. Embedding model converts documents into vectors at index time — stored in vector DB
> 2. Embedding model converts the user query into a vector at query time
> 3. Vector DB finds the closest document vectors to the query vector (similarity search)
> 4. The retrieved document chunks are passed as context to the generation model
> 5. Generation model (Claude, GPT-4) produces the final answer using that retrieved context
>
> **If using AWS Bedrock for a POC:** the natural embedding choice is Amazon Titan Embeddings G1, with Claude as the generation model. They are different models serving different roles in the same pipeline.

> **Rule:** Never say "we use Claude for embedding." Claude is used for generation. Embedding is a separate model. Know both.

---

## F6: AI code quality measurement — beyond productivity metrics

**Correction source:** Wells Fargo Round 2. When asked "besides time metrics, what else are you measuring from AI use?" the answer became circular and ended with "there is no way to calculate that." That is the wrong answer.

**Memory Hook:** First-submission pass rate → Rework rate → Escaped defect rate → Manual but tracked

> **Core Answer**
>
> Three code quality metrics I track specifically for AI-generated code, alongside the productivity metrics:
>
> **First-submission pass rate on code review.** When an engineer submits a PR for code that was AI-assisted, how often does it pass code review without rework on the first submission? I track this at sprint level for AI-assisted PRs versus non-AI PRs. If AI-assisted code requires significantly more rework, the prompting quality or the review process is not working.
>
> **Rework rate.** Of the code review comments received, what percentage require actual code changes versus are discussion points? If AI-generated code produces more mandatory rework comments, the model or the prompting template is generating low-quality code for that task type.
>
> **Escaped defect rate of AI-assisted features.** Of defects found in production or QA for features where AI-assisted code was used, how does the rate compare to the non-AI baseline? This is the most meaningful quality signal but has the longest feedback loop.
>
> **Honest acknowledgment:** these metrics are captured manually at sprint level, not automated. That is an honest gap. But saying "there is no way to measure it" is wrong — the measurement exists, it is just manual. Document the baseline before AI adoption starts so you have something to compare against.

---

## F7: Open source LLMs vs proprietary models — when to use which

**Memory Hook:** Proprietary for capability → Open source for control, cost, and data residency

> **Core Answer**
>
> The decision comes down to four factors: capability requirement, data residency, cost at scale, and operational control.
>
> **Proprietary models (Claude, GPT-4, Gemini):**
> Best-in-class capability for complex reasoning, code generation, and nuanced language tasks. Faster to integrate. Lower operational overhead — you consume an API, you do not manage the model infrastructure. The trade-off: data leaves your cloud boundary unless you use a managed deployment (Azure OpenAI, Bedrock, OCI GenAI with private endpoints). Also, cost per token adds up at high volume.
>
> **Open source models (Llama, Mistral, Falcon, Phi):**
> Data stays entirely within your infrastructure. No per-token cost once deployed — only the compute cost of running the model. You can fine-tune on domain-specific data without sending that data to a third party. The trade-off: you own the infrastructure, the model updates, the serving layer, and the performance tuning. Smaller models may not match proprietary capability on complex tasks.
>
> **When I would choose open source:**
> - PHI or PII in the prompts that cannot leave the cloud boundary under any circumstances
> - Very high query volume where per-token cost would be prohibitive
> - Need to fine-tune on proprietary domain data
> - Regulated industry requirement that the model must be self-hosted
>
> **When I would choose proprietary:**
> - Best capability is the priority and data can be contained via private endpoints
> - Speed to value matters — no model infrastructure to manage
> - Query volume is moderate and cost is manageable
>
> **Practical approach:** start with a proprietary model behind private endpoints for POCs — faster to validate the use case. If the use case proves value and volume or data residency constraints make open source the right long-term choice, plan a phased migration. Do not delay proving value to have a perfect open source deployment.

---

## F8: Redesign a legacy fraud detection system using AI — system design

**Memory Hook:** Data Foundation → Feature Engineering → Model Selection → Real-time Scoring Pipeline → Feedback Loop

> **Rule first:** When asked to design a system using AI, give an architecture, not an evaluation process. Do not describe how you would choose a model. Describe how the system works.

> **Core Answer — five layers:**
>
> **Layer 1: Data foundation and feature engineering.**
> Fraud detection lives or dies on features. Raw transaction data alone is not enough. Feature engineering extracts the signals that distinguish fraud from legitimate transactions:
> - Velocity features: how many transactions in the last 1 minute, 5 minutes, 1 hour, 24 hours for this card
> - Behavioural deviation: is this merchant category unusual for this cardholder? Is the transaction amount more than 3 standard deviations from their average?
> - Location delta: is the physical distance between this transaction and the previous one physically impossible given the time gap?
> - Time-of-day patterns: does this transaction occur at an unusual hour for this cardholder?
> - Network features: is this merchant or this IP associated with previously confirmed fraud?
>
> These features are computed in real time for each transaction and fed to the model.
>
> **Layer 2: Model selection.**
> For real-time fraud scoring, the model must be fast (milliseconds latency) and interpretable enough to explain why a transaction was flagged. Two approaches depending on the problem:
> - **Gradient boosting (XGBoost, LightGBM):** strong performance on tabular transaction data, fast inference, good feature importance for explainability. The standard baseline for fraud detection.
> - **Neural network / sequence model:** captures temporal patterns across a sequence of transactions — useful when the fraud signal is in the pattern of behaviour over time, not just a single transaction.
> - **LLM is NOT the primary scoring model for real-time fraud.** LLMs are for analysis, report generation, and anomaly explanation after the fact — not for millisecond scoring decisions.
>
> **Layer 3: Real-time scoring pipeline.**
> Transaction arrives → feature computation (from feature store, pre-computed where possible) → model inference → fraud score (0 to 1) → decision engine (block / flag for review / approve based on threshold) → response in under 100 milliseconds.
>
> Feature store is the key infrastructure piece — pre-computes and caches historical features (30-day velocity, 90-day behavioural baseline) so they are available in real time without querying raw transaction history at decision time.
>
> **Layer 4: Human review and feedback loop.**
> Transactions flagged but not blocked go to a review queue. Human analysts confirm or clear them. Every confirmed fraud label and every false positive goes back into the training data. The model is retrained on a scheduled cadence (weekly or monthly) with the latest labelled data. Without this feedback loop, model accuracy degrades over time as fraud patterns evolve.
>
> **Layer 5: LLM augmentation (where it adds value).**
> LLMs are useful for: generating natural language explanations of why a transaction was flagged ("this transaction was flagged because the merchant category is unusual for this cardholder and the amount is 4x their average"), summarising patterns in the fraud queue for analyst teams, and identifying emerging fraud patterns across the flagged queue that might not yet be in the training data.
>
> **One-sentence close:** *"The ML model scores in milliseconds, the feature store makes that possible in real time, the feedback loop keeps the model current, and the LLM adds explainability and analyst productivity on top of the core scoring system."*

---

*File 7 of 8 — GenAI, Agentic AI, AI Adoption & Productivity*
*Updated June 2026 — added F5 (embedding vs generation model), F6 (AI code quality measurement), F7 (open source vs proprietary LLMs), F8 (fraud detection system design using AI) from Wells Fargo Round 2*
