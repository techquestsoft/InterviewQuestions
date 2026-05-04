# AI / LLM Integration & Agentic AI Tools
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

> Note: This is a *Preferred* qualification per the JD. JPMC is actively investing in AI-enabled capabilities and agent-driven tools. Strong answers here can be a real differentiator.

---

## Q1: How would you integrate AI/LLM capabilities into customer-facing applications?

**Memory Trick:** Use Case → Architecture → Guardrails → Measure → Iterate

- **Identify high-value use cases** – Customer support assistance, document understanding for KYC, smart form filling, agent copilots. Start where AI clearly improves experience or efficiency.
- **Layered architecture** – API/access layer → orchestration → retrieval (RAG) → LLM → guardrails → observability. Decoupled so models and prompts can evolve independently.
- **Guardrails first** – Input validation, output filtering, PII redaction, content policies. Especially critical in banking.
- **Measure outcomes** – Accuracy, customer satisfaction, deflection rate, cost per request. AI without measurement is hype.
- **Iterate with feedback** – Human-in-the-loop, feedback collection, model and prompt refinement based on real usage.

---

## Q2: How do you design an end-to-end LLM-powered application?

**Memory Trick:** Access → Orchestrate → Retrieve → Generate → Guard → Observe

- **Access layer** – API gateway, auth, rate limiting, multi-tenant isolation.
- **Orchestration** – Prompt construction, tool/function calling, conversation state, workflow management.
- **Retrieval (RAG)** – Vector store (e.g., embedding store) for grounding answers in approved documents and data.
- **LLM layer** – Model abstraction (OpenAI, Anthropic, Bedrock, internal). Easy to swap without rewriting orchestration.
- **Guardrails + observability** – Input/output validation, prompt injection protection, full audit logging, cost and latency tracking.

---

## Q3: How do you decide between RAG, fine-tuning, and direct prompting?

**Memory Trick:** Context Need → Cost → Freshness → Compliance

- **Direct prompting** – Simple, well-defined tasks. Cheapest, fastest. Good starting point.
- **RAG (Retrieval-Augmented Generation)** – When you need answers grounded in proprietary or up-to-date documents (policies, KB articles, customer history). Easier than fine-tuning to keep fresh.
- **Fine-tuning** – When you need specific style, domain language, or behavior consistently. Higher cost, harder to update, may have compliance considerations.
- **Hybrid in practice** – Most enterprise apps end up combining: prompt engineering + RAG + occasionally fine-tuned smaller models for specific tasks.
- **Decide by cost and freshness** – RAG handles fresh knowledge better; fine-tuning bakes knowledge into the model and ages.

---

## Q4: How do you handle hallucinations and accuracy in LLM applications?

**Memory Trick:** Ground → Constrain → Validate → Human Review

- **Ground answers in retrieval** – RAG with high-quality, curated sources. Answers cite the source.
- **Constrain with prompts** – Clear instructions: "If you don't know, say so. Only answer from the provided documents."
- **Validate output** – Schema validation, fact-checking against known data, anomaly detection on confidence scores.
- **Human-in-the-loop for critical paths** – High-stakes decisions reviewed by humans before action.
- **Monitor in production** – Track flagged outputs, user thumbs-down feedback, accuracy sampling. Hallucinations don't stay constant.

---

## Q5: How would you drive adoption of agent-style tools and workflows for automation?

**Memory Trick:** Identify Toil → Pilot → Measure → Scale → Govern

- **Identify high-toil workflows** – Repetitive engineering tasks: PR reviews, log analysis, runbook execution, ticket triage. Agents shine in these.
- **Pilot with one team** – Start with a willing team, measurable workflow, clear success criteria.
- **Measure impact** – Time saved, error rate, engineer satisfaction. Build a real ROI story.
- **Scale with templates and guardrails** – Reusable patterns, approved tools, action-level audit logs.
- **Govern from day one** – Agents have access; access has consequences. RBAC, approval workflows, blast-radius limits.

---

## Q6: What reliability patterns do you apply to LLM applications?

**Memory Trick:** Timeout → Retry → Fallback → Cache → Cost Control

- **Timeouts and retries** – LLM APIs are slow and occasionally fail. Sensible timeouts, exponential backoff, jittered retries.
- **Fallback models** – Primary model unavailable? Failover to secondary. Avoid hard dependency on one provider.
- **Caching** – Semantic cache for similar queries. Reduces cost and latency dramatically.
- **Rate limiting and quotas** – Per-user, per-tenant. Prevents abuse and runaway cost.
- **Cost controls** – Token budgets, smaller models for simpler tasks, prompt optimization. LLM costs surprise teams.

---

## Q7: How do you handle security and compliance in LLM applications, especially in banking?

**Memory Trick:** PII → Prompt Injection → Audit → Data Residency → Responsible AI

- **PII handling** – Don't send sensitive customer data to external models. Mask or tokenize before LLM calls; use enterprise-isolated models when sensitive data is required.
- **Prompt injection protection** – Treat user input as untrusted. Separate system instructions from user input, validate, sanitize, and constrain tool calls.
- **Audit logging** – Every LLM call logged: who, what, prompt, response, model. Required for compliance and forensics.
- **Data residency and access control** – Models and data hosted in approved regions, isolated per tenant, with explicit data retention policies.
- **Responsible AI fundamentals** – Bias testing, explainability for customer-impacting decisions, human review for high-stakes outputs, kill switches.

---

## Q8: How do you measure success of an AI/LLM initiative?

**Memory Trick:** Adoption → Quality → Efficiency → Cost → Trust

- **Adoption** – Are users actually using it? Daily active users, queries per user, retention.
- **Quality** – Accuracy, customer satisfaction, escalation rate, user feedback scores.
- **Efficiency gain** – Time saved per task, deflection rate, throughput improvement.
- **Cost** – Cost per request, cost per resolved issue. AI must be cheaper than the alternative at scale.
- **Trust** – Hallucination rate, complaints, escalations, audit findings. Trust is the long-term success metric.

---

## Q9: How would you build an engineering team capable of delivering LLM-powered applications?

**Memory Trick:** Platform → Product → Governance → Upskill

- **AI platform team** – Owns shared capabilities: model abstraction, prompt management, RAG framework, evaluation tooling, cost monitoring.
- **Product teams** – Build use cases on top of the platform. Don't reinvent the LLM plumbing in every team.
- **Governance and security** – Dedicated reviewers for AI use cases, responsible AI checklists, security review.
- **Upskill existing engineers** – Most engineers don't need to be ML researchers. They need prompt engineering, RAG, evaluation, and reliability skills. Train and pair.
- **Hire selectively** – A few deep AI/ML hires for platform; the rest is upskilling existing strong engineers.

---

## Q10: Tell me about a real or planned use case where AI/LLM would improve customer experience in account origination or onboarding.

**Memory Trick:** Pain Point → Solution → Architecture → Measure

- **Pain point** – Customers abandon onboarding due to complex forms, document upload friction, and slow KYC verification.
- **AI-driven solution** –
  - Smart document understanding: LLM + vision model extracts data from uploaded IDs/docs, pre-fills forms, reduces typing.
  - Conversational assistant: answers customer questions in real time with grounded responses (RAG over policies and FAQs).
  - Intelligent routing: classifies edge cases for human review, with summarized context for the agent.
- **Architecture** – Document processor → entity extraction → form pre-fill API → guardrails (PII, accuracy threshold) → human fallback when confidence is low.
- **Measurement** – Onboarding completion rate, time to complete, customer satisfaction, agent handle time for escalations, accuracy of auto-extracted data.
- **Outcome target** – Faster onboarding, lower abandonment, lower cost per origination, while maintaining compliance and audit standards.

> Strong closing line: *"AI is a force multiplier when it's deeply integrated, well-governed, and measured. As an engineering leader, my job is to make sure we adopt it for real customer outcomes — not just because it's trendy."*

---
