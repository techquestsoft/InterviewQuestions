# FILE 9 — DELIVERY PLAYBOOK & MISSING WORKED PROBLEMS

> **Why this file exists.** Your content prep is strong. Across four real interviews (Evernorth, Ciklum, Trianz x2), the failures were almost never *knowledge* failures — they were **delivery failures in the first 60–90 seconds of an answer**: mishearing the question, answering before clarifying, leading with the wrong layer, and freezing on a term you actually know.
>
> This file trains the **reflexes** that fix that, then adds the two worked problems that came up live and weren't cleanly covered elsewhere (pull-based ingestion; agentic AI follow-ups).
>
> **Rule:** Content is necessary but not sufficient. The candidate who *leads* the conversation beats the one who *recovers* into it. This file is about leading from sentence one.

---

## PART 1: THE THREE DELIVERY FAILURES (DIAGNOSED FROM REAL TRANSCRIPTS)

### Failure 1 — Answering before clarifying
**Trianz:** Heard "200 PRs per minute" and immediately gave a vague API-layer answer. The interviewer had to correct the problem framing three times and supply the key constraint himself ("there's no push, it's pull").
**Cost:** 90 seconds of flailing before you found footing. You *had* the Kafka/DLQ answer — you just deployed it late.

### Failure 2 — Anchoring to your familiar domain instead of hearing the question
**Trianz:** Heard "PR" → thought "code review" (your daily context). The question was about *data ingestion*.
**Ciklum:** Asked about "your AI work" → led with React front-end and API gateway. They wanted the *agent*.
**Cost:** You answer a question they didn't ask, then have to be redirected.

### Failure 3 — Recovering into structure instead of leading with it
**Trianz:** Built the design reactively, layer by layer, as the interviewer prompted each piece (partitioning? consumers? no-Kafka alternative?). The substance was correct, but he was dragging it out of you.
**Cost:** Reads as "knows the parts but can't architect" — even when you can.

**All three share one root cause: you start before you've framed.** The fix is a single disciplined opening sequence, drilled until automatic.

---

## PART 2: THE UNIVERSAL OPENING SEQUENCE (DRILL THIS)

For ANY open-ended technical or design question, run these three moves **before** you design anything:

```
MOVE 1 — PLAYBACK   : "So the problem is X. Let me make sure I've got it."  (10 sec)
MOVE 2 — CLARIFY    : 2–3 sharp questions that change the design.            (20 sec)
MOVE 3 — FRAME      : "Here's the shape of my approach, then I'll drill in." (15 sec)
```

Only after these do you design. This sequence directly kills all three failures:
- Playback kills **anchoring** (you confirm what's actually being asked).
- Clarify kills **answering-before-clarifying** (and surfaces constraints like pull-vs-push yourself).
- Frame kills **recovering-into-structure** (you lead with the architecture, then drill).

### The script, filled in

**MOVE 1 — Playback:**
> "Let me play that back: you've got [scale/throughput], the goal is [X], and the part you want me to focus on is [Y]. Right?"

**MOVE 2 — Clarify (pick the 2–3 that matter):**
> "A few quick questions that'll change the design:
> - Is data pushed to us, or do we pull/poll the source?
> - What's the latency requirement — real-time seconds, or is near-real-time OK?
> - Can we lose any events, or is it zero-loss?
> - What's the read/write ratio and the peak vs. average?"

**MOVE 3 — Frame:**
> "Okay. At a high level I'd design this as [pollers → durable queue → consumers → failure handling]. Let me walk the end-to-end shape first, then drill into each layer."

That's ~45 seconds that converts every one of your "rocky-then-solid" performances into "solid throughout."

> **The single most important habit:** never draw a box before you've confirmed the problem and named your trade-off lens. Your own File 4 says it: *"Junior candidates draw boxes immediately."*

---

## PART 3: CLARIFYING QUESTIONS BY QUESTION-SHAPE

Memorize 2–3 per shape. These are the questions that actually change the architecture — asking them signals seniority AND buys you thinking time.

### Shape A — Ingestion / "capture all the events"
- Push (webhooks) or pull (poll)? **← the exact one you missed at Trianz**
- Zero-loss required, or is some loss tolerable?
- Real-time (seconds) or batch/near-real-time?
- Ordering required? Per-key or global?
- Peak vs. average throughput?

### Shape B — High-throughput / scaling
- Read-heavy or write-heavy? Ratio?
- What's the actual bottleneck today (or expected)?
- Latency SLO — p50, p99?
- Consistency need: strong, or eventual OK?

### Shape C — "Design system X" (greenfield)
- Who are the users, and what's the core use case?
- Scale: users, QPS, data volume?
- What are the hard constraints — existing systems, compliance, budget?
- What's the one NFR that matters most here — latency, availability, or cost?

### Shape D — AI / agentic
- What's the user actually trying to do in natural language?
- What data sources back the answer — structured, unstructured, both?
- Accuracy/safety bar? (In healthcare: grounding + hallucination control are non-negotiable.)
- Single workflow or genuinely independent sub-tasks? (Decides single-agent vs multi-agent.)

### Shape E — Behavioral / leadership (yes, clarify here too)
- For ambiguous ones ("tell me about a conflict"): "Do you want a cross-team conflict, or a people/performance one?" — lets you pick your strongest story instead of guessing.

---

## PART 4: "LISTEN FOR THE REAL QUESTION" — THE ANCHORING ANTIDOTE

Your pattern: a familiar word hijacks your interpretation. Train this 2-second check before answering:

| You hear… | Your reflex (wrong) | Pause and ask: what are they *actually* asking? |
|---|---|---|
| "PR" | code review | Could be *ingesting* PR data at scale (Trianz) |
| "your AI work" | describe the whole stack front-to-back | They want the *agent / the hard part* (Ciklum) |
| "migration" | your K8s story on autopilot | Could be data migration, cloud, or org change |
| "scale" | infrastructure | Often *operational + team* readiness (Availity note in File 4) |
| "tough stakeholder" | general philosophy | They want ONE specific story with an outcome |

**The drill:** when a familiar keyword lands, take one breath and silently ask *"what's the actual question?"* before the first word leaves your mouth. One breath. That's the whole fix.

---

## PART 5: WORKED PROBLEM 1 — PULL-BASED PR INGESTION (THE TRIANZ QUESTION)

This is the exact question you got. Your File 4 covers *push/event-driven* Kafka well, but this one is specifically **no push — you must pull**. Different shape. Here's the clean, lead-from-the-front version.

**The question:** An application ingests pull requests from many open-source Git repos. ~200 PRs/min average, peak 500. Capture *all* of them (zero-loss), near-real-time, for downstream processing. There is **no webhook/push** — the app must poll. Design the ingestion.

### Step 0 — Opening sequence (don't skip)
> "Playback: many repos, ~200/min average, 500 peak, zero-loss, near-real-time, and since there's no push I have to poll the source. Quick checks: roughly how many repos? Do we have a reliable cursor per repo — a last-seen PR id or updated-timestamp — so I can poll incrementally? And once captured, does downstream need ordering per repo?"

*(Asking the pull-vs-push and cursor questions yourself is exactly what you didn't do live. Do it here and you've already won the question.)*

### Step 1 — Frame the shape
> "High level: scheduled pollers per repo → write each PR to a durable queue (Kafka) immediately → consumer groups process downstream → failure handling via retry + DLQ. The durable queue is the zero-loss backbone. Let me walk each part."

### Step 2 — The poller (ingestion)
- A scheduled job (cron / Spring `@Scheduled` / a poller service) queries each repo's API for PRs newer than the **last-seen cursor** (PR id or `updated_at`).
- Store the cursor per repo in a small state store. On each run, advance it only after the PRs are safely written to Kafka.
- 200/min is tiny; parallelize pollers across repos with a worker pool. Stagger schedules to avoid thundering-herd against the source API.
- **Respect source rate limits** (GitHub-style APIs throttle): exponential backoff on 429s, honor `Retry-After`.

### Step 3 — The durability backbone (zero-loss)
- Each fetched PR is published to a **Kafka topic immediately** on capture. This is what guarantees no loss: even if downstream is slow or crashes, events sit durably in Kafka and consumers resume via offsets.
- **Advance the cursor only after the Kafka write is acknowledged** (`acks=all`). If the write fails, don't advance — next poll re-fetches. This makes capture **at-least-once**; idempotency (Step 5) makes it safe.

### Step 4 — Partitioning & consumers
- **Partition by repo** (repo id as key) → preserves per-repo ordering and spreads load. (This was your answer live — it's correct.)
- **Consumer group sized to partitions** — N partitions, up to N consumers for parallel reads. Fewer → backlog; more → idle consumers. (Also your live answer — correct.)
- Consumers commit offsets **only after successful processing** → crash mid-process re-delivers rather than drops.

### Step 5 — Failure handling (your strongest live moment — keep it)
- **Retry with exponential backoff**, ~3 attempts.
- Then route to **DLQ**, split **transient vs permanent**:
  - Transient (timeout, throttle) → reprocess later, succeeds.
  - Permanent (schema, auth) → ops triages from DLQ.
- **Idempotent processing keyed on PR id** → at-least-once delivery can't create duplicates.
- Alert on **consumer lag** and **DLQ growth**.

### Step 6 — The "no Kafka allowed" variant (they asked this)
> "If Kafka's off the table: pollers write captured PRs to a durable store — a table or queue — and a scheduled worker processes them, advancing a status flag (PENDING → DONE → ERROR). Failures land in an error table reprocessed on a schedule. It's the same shape — durable buffer + decoupled processing + retry — just without Kafka's built-in partitioning and replay, which I'd now have to build myself."

### Step 7 — DB choice (they pushed on this — here's the cleaner answer)
You got tangled on NoSQL-vs-OLTP live. Clean version:
> "For an append-only capture log with no CRUD and no strong-consistency need, either works. The honest deciding factor at this volume isn't capability — 200–500/min is trivial for PostgreSQL — it's **operational simplicity**. I'd default to a managed relational DB (Postgres) because it's lower-overhead to run than a distributed NoSQL cluster. I'd only reach for NoSQL if volume or write-fan-out grew enough that horizontal scale and schema flexibility actually paid for the added ops cost. The trade-off is operability, not raw throughput."

*(This is the point the interviewer was steering you toward — lead with it instead of being led to it.)*

### Memory hook for this whole problem
**Poll with a cursor → Kafka for zero-loss → partition by repo → consumer group → retry/DLQ → idempotent by PR id.**

---

## PART 6: WORKED PROBLEM 2 — AGENTIC AI FOLLOW-UPS (THE CIKLUM QUESTION)

Condensed from your dedicated agentic prep — the highest-value pieces, so this file is self-contained. Use the full agentic sheet for depth.

### The framing that wins
Your real system = **a single orchestrator agent with multiple tools and a RAG pipeline, with grounding and output validation.** Say that sentence and most follow-ups are pre-answered.

### Lead with the agent, NOT the plumbing
When asked "tell me about your AI work," open with the agent and its decision logic. Mention React / API gateway **last, briefly**. (Live, you led with plumbing and got interrupted.)

> "We built an agentic conversational system for care managers. They ask in natural language — 'show me this week's top-10 readmission-risk patients' — and an orchestrator agent interprets intent, decides which backend tool to use, retrieves the data, and returns a grounded answer. It routes between a structured store for scores and a RAG pipeline for clinical explainability — so it can also answer *why* a patient's risk is high, citing their actual conditions."

### The exact follow-ups, pre-answered
- **"Multi-agent?"** → "No — to be precise, a single orchestrator agent with multiple tools, not multi-agent. It could extend to multiple agents (e.g., a dedicated clinical-reasoning agent), but for one clear workflow a single orchestrator was the right complexity."
- **"Orchestrating agent?"** → "Yes. It orchestrates the flow: input validation → LLM intent reasoning → tool selection → sequencing calls when a query needs both score data and clinical context → output validation for grounding."
- **"Behind the scenes?"** → agent logic first (validation → LLM tool-selection → OpenSearch tool vs RAG tool → LangChain orchestration → grounding/output validation), *then* the infra in one line.
- **"Prevent hallucination?"** → "Three layers: grounding via prompt design, RAG-backed answers from retrieved clinical docs, and output validation that declines when the answer isn't in our knowledge base. In healthcare, a confident wrong answer is worse than 'I don't know.'"

### Never again
- Never say **"what do you mean?"** to a standard term. If unsure: *"If you mean X, then yes — here's how mine does it."*
- Precision beats inflation: "single-agent, and multi-agent would've been over-engineering" reads as **judgment**. Vague "multi-agent" claims collapse under follow-up.

---

## PART 7: THE RECURRING SMALL FIXES (STILL COSTING YOU)

### The name — drill to automaticity
It garbled in **every** interview. This is the cheapest possible fix and it keeps recurring.
> "Rajasekhar Reddy Yakkaluru — I go by Sekhar."
Say it as three deliberate chunks: **Rajasekhar** · **Reddy** · **Yakkaluru** · "I go by Sekhar." 30 reps in isolation before any interview. Same for **"Oracle Cerner"** (came out "Oracle seminar / Veracl / Oracle Center").

### Don't trail off — land every answer
You end answers fading ("…evolve over the period of time," "…if you want I can give another example"). Replace with one crisp takeaway:
> "The lesson I took from this was [X]." — then stop.

### Lead with the direct answer on "are you comfortable with X"
Live, you explained strategy first and the commitment got lost; they had to re-ask. Fix: **"Yes, I've done this — here's the evidence,"** then explain.

### Stop volunteering extra stories
"Actually I have another example if you want" signals you're unsure you answered. Say your piece, stop, let them ask.

### Strip the acronym storm
"CVE for JSON bindings requiring Java 17 upgradation" → "a security vulnerability needing a Java version upgrade." Lead with the decision; use detail only to anchor credibility.

---

## PART 8: THE 10-MINUTE PRE-INTERVIEW DRILL

Do this, then stop — don't over-rehearse into fatigue (that's when new errors appear).

1. **Name + employer**, in isolation, until automatic:
   "Rajasekhar Reddy Yakkaluru, I go by Sekhar." / "Oracle Cerner." — ~10 reps.
2. **The opening sequence** (Playback → Clarify → Frame) — say it once against a made-up design prompt.
3. **Two anchor sentences:**
   - Ingestion: "Poll with a cursor → Kafka for zero-loss → partition by repo → consumer group → retry/DLQ → idempotent by PR id."
   - Agentic: "Single orchestrator agent with multiple tools and a RAG pipeline, with grounding and output validation."
4. **One breath rule:** remind yourself — when a familiar keyword lands, one breath, ask *"what's the actual question?"* before answering.

---

## PART 9: ONE-PAGE CHEAT SHEET

```
OPENING SEQUENCE (every design/technical Q)
  1. PLAYBACK  — "Let me play that back…"
  2. CLARIFY   — 2–3 questions that change the design
  3. FRAME     — "Here's the shape, then I'll drill in"

KILLER CLARIFIERS
  Ingestion : push or pull? zero-loss? real-time? ordering?
  Scaling   : read/write ratio? bottleneck? p99 SLO? consistency?
  Greenfield: users? scale? hard constraints? top NFR?
  AI        : structured/unstructured? accuracy bar? one workflow or many?

LISTEN FOR THE REAL QUESTION
  familiar keyword → one breath → "what are they ACTUALLY asking?"

PULL INGESTION HOOK
  poll w/ cursor → Kafka (zero-loss) → partition by repo →
  consumer group → retry/DLQ → idempotent by PR id
  (no-Kafka: durable table + scheduled worker + error table)
  (DB choice: default Postgres for operability, not throughput)

AGENTIC HOOK
  single orchestrator agent + multiple tools + RAG +
  grounding + output validation
  lead with the AGENT, not the front-end
  never say "what do you mean?" → "if you mean X, then yes…"

ALWAYS
  name slow & clean · land the takeaway · direct answer first ·
  don't volunteer extra stories · strip acronyms · don't over-rehearse
```

---

*File 9 of 9 — Delivery Playbook & Missing Worked Problems. Pairs with File 4 (System Design) and the STAR Stories file. The content lives in those; this file makes sure it comes out right under pressure.*
