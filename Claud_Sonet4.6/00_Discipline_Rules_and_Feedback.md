# 00 — Discipline Rules, Hard-Won Feedback & Interview Patterns

> **Read this before every interview. Not the technical files. This one.**
> Everything here comes from what actually happened — Availity, Deloitte, Cubic, TJX, Wells Fargo, HighRadius, Globalogic, Optum, Evernorth.

---

## THE SINGLE BIGGEST RISK RIGHT NOW

It is not a technical gap. It is a discipline gap.

TJX proved you can deliver senior-EM-level answers when you are focused.
Cubic proved one undisciplined sentence can erase 45 minutes of good content.

> *"I laid off in Oracle, so I need a job, I don't know either I can tell or can I put some story on that, but reality is that one."*

That sentence did three things in 25 words: volunteered a layoff that wasn't asked, signalled desperation, and suggested you might fabricate. That is the conversation ending — no technical recovery saves it. This must never happen again.

---

## PART 1 — THE NON-NEGOTIABLES (HARD RULES)

### 1. Never volunteer your layoff status

If not asked: say nothing.
If asked why you are leaving: *"Oracle went through organisational changes that affected my team. I am using this as an opportunity to find a role where I can take the next step — building AI-embedded engineering teams at scale."*
If asked directly whether you were laid off: *"Yes, my role was impacted by a restructuring. It was not performance-related — the team delivered the V3 ML transformation and the OCI migration on track. I am being deliberate about what I do next."*

**Never say:** "I need a job." "I don't know if I should tell you." "Maybe I can put a story on that."

### 2. Never give compensation or notice period details in a screening call

Wait for the compensation discussion stage. If pressed: *"I am flexible — happy to share once I understand the full role scope and compensation structure."*

### 3. Introduction is 90 seconds. Stop.

Every interview — Availity, Deloitte, Wells Fargo, Cubic, Globalogic — you ran 5 to 8 minutes. Interviewers explicitly said "you are explaining too much." Set a timer. 90 seconds. Stop and wait for questions.

### 4. Lead management questions with management. Technical is the last third.

"As a manager, what do you do when the payment service is down?" does not want a Saga pattern answer. It wants: blast radius → communicate → mitigate → root cause → CAPA. Saga comes at the end as future prevention, if at all.

### 5. Never say "give me time to think" for basic EM KPIs

MTTD, MTTR, Change Failure Rate, Escaped Defect Rate, Delivery Predictability — these should come immediately. Asking for time on a standard executive KPI question signals you have not operated at that level. Memorise both sets cold.

### 6. Never claim outcomes your team did not own

Santosh at Deloitte caught this directly: you said "40% accuracy improvement from ML vs rule-based" but acknowledged the data science team did the work. Fix: *"The data science team achieved 40% accuracy improvement. My team's contribution was the integration architecture, the delivery coordination, and the migration rollout."*

### 7. Never say "I am not able to recollect" for standard technical topics

Lambda use cases, rolling update, memory leak causes — these are not edge cases. They are baseline senior-EM knowledge. If you genuinely blank, say: *"Let me think through this properly"* and take 5 seconds. That is confidence. "I cannot recollect" is a red flag.

---

## PART 2 — WRONG ANSWERS FROM ACTUAL INTERVIEWS (CORRECTED)

### Sharding definition — Cubic (WRONG)

**What you said:** "Sharding means within the partitioning, how many shards are required."

**Why it's wrong:** Partitioning and sharding are different levels.

**Correct answer:**
> Partitioning is dividing data within one database instance by a logical key — tenant ID, date range. Reads for one partition don't touch another.
>
> Sharding is distributing those partitions across multiple physical database servers. Each shard is a separate DB instance holding a subset of the total data. No single instance holds everything, so no single instance is the bottleneck. The complexity comes from cross-shard queries — you need a routing layer to determine which shard holds the data for any given key.

---

### Rate limiter — Optum OCM (WRONG)

**What you said:** Used a static variable to track request counts per consumer.

**Why it's wrong:** Static variables are per-JVM process. In Kubernetes, each pod has its own memory. A static counter enforces nothing globally across pods — every pod counts independently.

**Correct answer:**
> For a global per-consumer rate limit across multiple Kubernetes pods, the counter must live outside the JVM — in Redis. Use an atomic increment on a Redis key that is namespaced by consumer ID and has a TTL equal to the rate window. A sliding window or token bucket algorithm against that Redis key gives you a genuinely distributed, per-consumer limit that survives horizontal scaling.

---

### Memory leak causes — Cubic (WEAK)

**What you said:** "Not utilising variables properly. Not utilising loops."

**Correct causes:**
> Cache without TTL or LRU eviction — grows indefinitely in heap.
> ThreadLocal values not removed after request completion — accumulate per thread.
> Static collections holding object references — JVM cannot garbage collect them.
> Unclosed DB connections or streams — connection pool exhaustion over time.
>
> Fix pattern: Redis with TTL for caching, try-with-resources for streams, WeakReference or explicit cleanup for ThreadLocal.

---

### MCP role confusion — Availity (WRONG)

**What you said:** Described MCP inserting data into the database.

**Correct position:**
> MCP is a connectivity and context protocol — it provides the LLM with tools to call external services. It does not write to databases directly. The LLM uses MCP tooling to query OpenSearch, and the results are formatted and returned. Data writes happen through your normal API layer, not through MCP.

---

### RAG inconsistency — Deloitte (INCONSISTENT)

You said RAG is not needed, then said it might be needed, then mentioned LangGraph. Pick one clean position:

> For structured data in OpenSearch: K-NN vector search is sufficient — no separate RAG pipeline needed.
> RAG is the right pattern when the source is unstructured documents not in OpenSearch — discharge summaries, clinical notes, PDF reports. In that case you embed those documents, store vectors in a vector DB, and use RAG to retrieve relevant context before the LLM answers.
> LangGraph handles multi-step conversational orchestration — separate concern from retrieval pattern.

These three things are not in conflict. They serve different purposes.

---

### Notification service callback — Availity (INCOMPLETE)

**What you said:** Good high-level design but no clean answer on "how do you tell the caller success or failed?"

**Complete answer:**
> Producer includes a correlation ID and a reply-to Kafka topic in the request. On completion — success or failure — the notification service publishes the result to that reply-to topic, keyed by correlation ID. Producer subscribes and matches on its own messages.
>
> For synchronous callers: expose a polling endpoint — GET /notifications/{correlationId}/status — returning PENDING, IN_PROGRESS, DELIVERED, FAILED, DEAD_LETTERED.
>
> Permanent failures after retries go to a dead letter queue with error code and reason. Operations is alerted. A replay endpoint exists for manual intervention.

---

### Consumer schema break — Optum OCM (TANGLED)

**Correct, clean version:**
> Adding a new optional field to a JSON response never breaks a consumer — existing fields are still there, and the new field is simply ignored if the consumer doesn't bind it.
>
> What breaks a consumer is renaming or removing a field. If `firstName` becomes `first_name`, any consumer using `getFirstName()` or mapping `firstName` in their DTO now gets a null or a deserialization error. That is a breaking change — it requires versioning, a transition period with both names present, or coordinated consumer upgrades.

---

## PART 3 — VOICE AND DELIVERY PATTERNS

### Your natural voice — use it

The best moments in your interviews (TJX low-performer story, Deloitte CAPA conversation, Optum OCI APM conflict) had a pattern: you gave a real example, you said what you actually did, and you drew one reflection from it. That is your voice. Use it.

The worst moments had hedging: "honestly I do not have the answer," "give me time to think," "either I can tell or put a story on it." Strip all of this.

### Claim → Mechanism → Example. Always.

Most of your technically correct answers arrived in the wrong order. You gave the example first, then the principle. Or the principle twice and forgot the mechanism.

Format for every technical answer:
1. **One-sentence claim** — what you believe or do
2. **Mechanism** — how it works / why it works that way
3. **Your example** — what actually happened in your work

Example of this done right (from your TJX interview):
> "I hold 15–20 percent of every sprint for tech debt. The mechanism is simple: it is in the sprint plan before feature work is discussed, not negotiated away afterward. The example is our Java 17 upgrade at Cerner — by treating it as a standing allocation rather than a special request, we completed it in two sprints without disrupting the feature roadmap."

That structure is natural, clear, and sounds like you — not like a prep document.

### Numbers consistency — use these, always

| Number | Context |
|--------|---------|
| 20+ years total experience | Intro |
| 22 years | Globalogic context (their tracking) |
| $5M annual savings | Optum Kubernetes |
| 110+ microservices | Optum migration scope |
| $10M annual savings / 20% fraud reduction | Bank of America Hadoop |
| 20–40% credit loss reduction | BoA decision engine |
| 120+ customers | Cerner Care Coordination |
| 14 engineers / 2 scrum teams | Cerner team (exact) |
| ~40% accuracy improvement | V3 ML — data science achieved it, my team integrated and delivered |
| >95% SLO maintained | Cerner operational record |
| 47-minute MTTR | Kafka replay incident — actual number |
| 20% productivity improvement | AI-assisted dev over 15 months |

**Resume says $5M, not $9M. 110+ services, not 600. 120+ customers, not 180.** Use the resume numbers. Inconsistency destroys credibility.

---

## PART 4 — WHAT WORKS BY INTERVIEW TYPE

### Technical screen (Availity R1, Cubic, Optum OCM)

Lead with: system design framework → claim → mechanism → example.
Length per answer: 60–90 seconds unless they ask for more.
What they are checking: do you know the patterns, do you know when to use them.

### Management/leadership round (Availity R2, Deloitte R1, TJX)

Lead with: the management action first — team, communication, decision. Technical is the last third.
Length: STAR story, 90 seconds, hard stop.
What they are checking: do you think like a leader or like an architect.

### Director/VP-level / AD-level (Deloitte R2, Evernorth AD, Globalogic R3)

Lead with: business outcome, P&L language, organisational scale.
What they are checking: can you operate at the level of strategy, not just execution.
Frames to use: "the business case was…", "the margin impact was…", "the commercial risk was…"

### Competency-based behavioral (Optum R3 with Srikanth, Evernorth AD panel)

Format: STAR, strict.
Length: 90 seconds spoken. Not 5 minutes.
End every STAR with a one-sentence reflection — "the lesson I carry is…" — this is the AD-level signal.

---

## PART 5 — TOPICS WITH CLEANED-UP ANSWERS (QUICK REFERENCE)

### Lambda — three use cases (memorise)

1. Event-driven trigger: S3 object creation fires Lambda to kick off pipeline validation and routing
2. Glue code between pipeline stages: Lambda checks EMR job exit status and either advances the pipeline or routes to error handling
3. SLA monitoring: Lambda on CloudWatch schedule — if a job has not completed within its SLA window, Lambda pages on-call and posts to Slack

### Rolling update (basic — must not stumble on this)

> Replace pods gradually — for example, 2 of 10 at a time. New pods come up, pass readiness checks, then old pods come down. Zero downtime but slow rollback — if the new version is bad, you wait for the gradual replacement to complete or accelerate it. Suitable for routine, low-risk releases.

### Active-active database failure mid-transaction (TJX — clean version)

> If the local DB write and the outbox table write don't both succeed, the transaction does not commit — @Transactional ensures atomicity on the local side. The user sees a failed request and retries. Traffic routes to Zone B on retry. Zone B completes the transaction. The outcome is brief unavailability for that user — not data inconsistency. Data loss is the risk you design around; brief unavailability is the acceptable trade-off.

### Why New Relic over OCI APM (your real experience)

> I ran a maturity analysis comparing both — distributed tracing, alerting flexibility, dashboard integration. OCI APM had real feature gaps for our observability needs at that time. Rather than switch and lose SRE quality, I negotiated with the platform team to defer migration until OCI APM matured. They accepted the analysis and used it to prioritise OCI APM improvements. Migration happened the following quarter — smoother for every team that came after us.

---

## PART 6 — AD-LEVEL FRAMING (EVERNORTH / GLOBALOGIC)

At AD or Director level, every answer should ladder to one of these three:

1. **Risk and compliance first** — in healthcare, compliance trumps speed. Non-negotiable.
2. **Value creation over cost arbitrage** — move the India team from order-takers to end-to-end owners.
3. **Commercial acumen** — speak P&L: margins, attrition cost, time-to-market, tech-debt carrying cost.

### How to talk about managing margins (Q they will ask at AD level)

> "Margin pressure in Hyderabad is real with rising talent costs. I work it on three levers. First, the team pyramid — rebalance so senior engineers are not doing tier-2 support work. Second, automation — every repetitive manual step in deployment, reporting, or support gets automated, freeing capacity for value-adding work. Third, retention — every senior exit costs 6 to 9 months of ramp. Keeping good people through meaningful work and real growth directly protects margin more than any other lever."

### How to frame India as a centre of excellence (not a delivery hub)

> "The shift I drive is from extension model — US defines architecture, India writes code — to genuine end-to-end ownership. Three moves: build domain depth so engineers understand the clinical or business why, not just the technical what. Shift ownership one capability at a time — one module, full lifecycle, prove it before scaling. Embed India in upstream conversations — product planning and architecture — so the team shapes requirements rather than receives them. At Cerner, my India team owned two products end to end. 120-plus customers. $20M+ revenue. That is the model."

---

*Last updated: June 2026 | Read before every interview*
