# Wells Fargo — Interview Feedback
**Round 1:** Swetha (Data Engineering Manager — hiring manager)
**Round 2:** Ramakrishna (Swetha's reportee)
**Date:** June 2026
**Based strictly on transcript — no assumptions**

---

## OVERALL VERDICT

**Round 1 with Swetha:** Strong. You answered almost everything well. A few areas where depth was missing at the right moment, but the overall impression was clearly positive — she asked you back for Round 2 within the hour.

**Round 2 with Ramakrishna:** Weak. Several answers were incomplete or evasive. The Kafka answer in particular was a low point. The closing — "No, I don't have anything" when asked for questions — was flat. This round likely created doubt that Round 1 had not created.

---

## ROUND 1 — SWETHA

### Introduction
**What you did:** 22 years, HSBC 3.5 years, BoA 11 years, Optum 6 years, Oracle Cerner 22 months.

**Problem 1 — The number.**
You said "22 years" again. Your resume says 20+ years. This has been flagged before. Kanbay was Feb 2004. Today is June 2026. That is 22 years and 4 months. But you have been saying "20+ years" in prep — the intro now says "22 years." Pick one and use it consistently. The interviewer called you "Shankar" for a while — probably because the name went by quickly in a long sentence.

**Problem 2 — HSBC is not on your resume.**
Your resume starts at Kanbay, then BoA. You mentioned HSBC in the intro. If HSBC is not on your resume, do not mention it in the intro — it creates confusion and an unverifiable claim.

**Problem 3 — The intro ran long.**
She asked for "5 to 8 minutes focused on the data aspect." You gave a full career walkthrough. That is fine for 5 to 8 minutes — but it covered everything including the structure of scrum teams and DevOps rotation, which is not data-focused. Stay on the data thread when explicitly asked.

---

### Cloud Migration — OpenShift to Kubernetes and AWS to OCI
**What you did:** Very strong. The layered approach (networking/IAM → compute → data → observability), the phased migration philosophy, the parallel-run for the first 5 customers, the observability-first discipline — all of it was well told. The authorization service transient spike example was a good, specific detail.

**What was missing:**
When she pressed on the SLA scenario ("data has to be there by 5 AM") she asked about the parallel-run specifically. You answered it — yes, parallel for AWS-to-OCI data — but you briefly confused yourself between microservices traffic flip and data parallel-run. She caught it and you corrected. Minor but visible.

**Grade: 8.5/10**

---

### SLA Breach Scenario (5 AM deadline, team still investigating)
**What you did:** This was your best answer in Round 1.

Alert at 70% of the SLA window. Engage the risk executive at 30% buffer remaining. Communicate every half hour. Two-track approach: fix AND communicate in parallel. The CVE backlog scenario — "we deprioritized it, now it is causing the failure, be transparent." The upstream-failure scenario — transparent to leadership, copy-forward from the last snapshot, communicate what two attributes won't be refreshed.

You got to the copy-forward / point-in-time snapshot answer on your own before she suggested it. That showed real operational thinking.

**One missed beat:**
When she said "what is in your control?" and cut you off from the upstream conversation — you paused and then got back on track. The pause was fine. But your answer before the cut-off was getting tangled in upstream stakeholder management which she explicitly said to put aside. When she redirected, you adapted well.

**Grade: 9/10**

---

### Small Part File Issue
**What you did:** Honest. "Recently we didn't face this issue. At BoA with MapReduce we had it — we set a minimum file size threshold of 120 MB." You acknowledged the technology has evolved.

**What was missing:** Nothing critical. She moved on. Clean answer.

**Grade: 8/10**

---

### GenAI — Productivity and Use Case
**What you did:** Well structured. Oracle Code Assist, 15 months, structured prompting templates, .md files, tracking velocity + delivery time. The readmission prevention POC — conversational UI, open search tool, RAG for clinical documentation, system prompting, persona definition, three-level validation. The Oracle Health Life Sciences Review process for governance.

**Problem — the ML accuracy claim:**
You said "increase the accuracy by 20%... say 50 to 20 that is 70 percent." The data science team achieved this. You led the partnership and proposed the initiative. In Round 2, Ramakrishna read "engineering productivity improved by 20% using AI" from your resume and you correctly clarified that is Oracle Code Assist productivity. But across both rounds, there is a risk that the ML accuracy improvement sounds like your technical achievement. State clearly: "The data science team achieved the accuracy improvement. My role was proposing the partnership, leading the integration effort, and governing the rollout."

**Grade: 8/10**

---

### DevOps Structure
One DevOps rotation per sprint within the scrum team of six. Clean, direct answer. No issues.

**Grade: 9/10**

---

### Budget Management
**What you did:** Honest — not doing budget at Cerner, did it at Optum. Explained the vendor strategy (Persistent, CTS, Wipro), phased reduction as migration completed, 25 people at peak down to 8-10, vendor governance through CI/CD gates and KT guardrails.

**What was missing:** She asked "did you have to reduce people costs?" You answered the vendor strategy but did not directly answer whether Optum headcount was reduced or whether internal people were impacted. The answer implied you planned for vendor reduction from the start, which is a clean answer — but it sidestepped whether any internal headcount decisions happened. That is fine — just note she may follow up on this.

**Grade: 7.5/10**

---

### Vendor Accountability + US Stakeholder Engagement
**What you did:** Strong on vendor accountability — CI/CD gates, 90% coverage, Fortify, OWASP, two-level PR approval, vendor onboarding to the pipeline. Strong on stakeholder structure — CCB/Feature Review weekly, DevOps review weekly, leadership meeting bi-weekly. The hybrid team management (bi-weekly sync, reviewing their Jira data before the call, raising recognition gaps) was a mature answer.

**Grade: 9/10**

---

### Questions You Asked
You asked about the role, the success criteria, the stakeholder structure. Solid questions. She gave you a genuinely detailed answer about the NexusOne/data private cloud migration, AI adoption, and the product-tech merge. You listened and asked a follow-up about India vs US stakeholders. Good.

**Round 1 overall: Strong. Estimated grade: 8/10**

---

## ROUND 2 — RAMAKRISHNA

### Self-Rating: Data 7-8, AI 5-6
Fine. Honest framing. No issue.

---

### AI Productivity — Measuring the 20%
**What you did:** You explained Oracle Code Assist, the structured .md prompts, tracking sprint velocity and delivery time-to-production as two parameters.

**Where it went wrong:**
Ramakrishna asked directly: "Besides time-related metrics — velocity, story points, days — what else are you measuring? Anything on code quality?"

Your answer became circular and long. You said code quality is a separate metric, not the productivity metric. You explained the two-level PR review, rework tracking, AI-generated code smells. Then you said: "There is no way of calculating that one. That is a manual work."

**The problem:** "There is no way of calculating it" is a weak answer for a Senior Engineering Manager being asked about measurement rigor. You had the right instinct — track rework rate, escaped defect rate of AI-generated code, code review pass rate on first submission — but you buried it under too many qualifications and then undermined it by saying it cannot be measured.

**What you should have said:** "We track two quality-specific metrics for AI-generated code: first-submission pass rate on code review, and rework rate compared to our non-AI baseline. Both are captured manually at sprint level. We have not automated this yet — that is an honest gap — but the data is visible at retrospective."

**Grade: 5/10**

---

### Fraud Detection Redesign Using AI
**What you did:** Synthetic data → three models (Claude, OpenAI, Gemini) → human-in-loop to compare accuracy → pick the best model.

**What was missing — this was a significant gap:**
Ramakrishna said explicitly: "I want you to talk specifically about how you will implement it technically. Walk me through the technology stack." 

Your answer was entirely at the process level — synthetic data, three models, human-in-loop evaluation. You never discussed:
- What type of ML model is appropriate for fraud detection (anomaly detection, gradient boosting, neural network for sequence patterns)
- Feature engineering for transaction data (velocity, location delta, merchant category, time-of-day patterns)
- Real-time vs batch scoring architecture
- How the model plugs into a transaction processing pipeline
- Threshold tuning and false positive management

He was asking you to design a system. You described an evaluation process. These are different things. He let it go, which means he either was satisfied (unlikely given the directness of his ask) or moved on because the answer was not going where he wanted.

**Grade: 4/10**

---

### Kafka in C360 — Why Kafka
**This is the worst answer in both transcripts.**

He asked: "How did you use Kafka here? Why Kafka, not a batch application?"

Your answer: "For incremental loads, we are using Kafka."

He pushed: "If it is just incremental load, you can do that without Kafka. Why Kafka specifically?"

Your answer: "Because of that is the incremental processing using the streaming, it will get only the incremental records for us."

That is circular. You defined Kafka using Kafka's function without explaining the actual reason.

**What the answer should have been:**
"We chose Kafka over batch because of three specific requirements. First, near-real-time patient data updates — when a risk score changes, the care manager UI needs to reflect it within minutes, not the next morning's batch. Second, multiple consumers — the same patient event needed to trigger the risk scoring engine, the notification service, and the audit log simultaneously. With Kafka, each consumer reads independently without the producer knowing about them. Third, fault tolerance and replay — if the downstream risk scoring job failed, we could replay from the committed offset rather than re-extracting from the source database. These three things together made Kafka the right choice over a scheduled batch job."

**Grade: 2/10**

---

### Chunking Strategy for RAG
**What you did:** You described chunking by 120 clinical parameters per patient — each parameter (like a comorbidity code with its description) as one chunk. That is a domain-specific, defensible chunking strategy.

**What was weak:** When he asked "how did you decide how to chunk?" your answer was long and circular before arriving at "120 parameters, one chunk each." Lead with the decision, then explain.

He also asked: "What model are you using to embed the documents?" Your answer: "We are using the Claude only... Opus 4.3."

**Problem:** Claude (Anthropic) is not typically used as an embedding model. Embedding models are separate from generation models — typically `text-embedding-3` from OpenAI, `embed-english-v3.0` from Cohere, or similar. If you used Claude Opus for generation and a separate embedding model, you needed to distinguish those two things. Ramakrishna moved past this, but it suggested confusion between generation and embedding models.

**Grade: 5/10**

---

### Questions at the End
He asked: "Any questions for me?"

Your answer: "No, I don't have anything."

**This is a significant mistake.** Never close a technical round with no questions, especially with the interviewer's direct report. You had just heard (in Round 1) that Swetha's team is migrating from Hadoop/Hortonworks to NexusOne on data private cloud. You could have asked:

- "Swetha mentioned the migration from Hortonworks to NexusOne is in progress — what does the current data validation workflow look like and where is the biggest pain point right now?"
- "What does the tech stack look like for the existing data pipelines — are teams using Spark on the cluster, or is there already movement toward managed services?"
- "What does success look like in the first 90 days for this role from your team's perspective?"

No questions signals either disinterest or lack of curiosity. At your level, it will be noticed.

**Grade: 1/10**

---

## SUMMARY TABLE

| Area | Round 1 (Swetha) | Round 2 (Ramakrishna) |
|---|---|---|
| Introduction | 7/10 — "22 years", HSBC confusion | N/A (brief) |
| Cloud migration story | 8.5/10 — strong and specific | N/A |
| SLA breach scenario | 9/10 — best answer overall | N/A |
| Small part file | 8/10 — honest, clean | N/A |
| GenAI productivity | 8/10 — structured | 5/10 — measurement answer weak |
| GenAI architecture | 8/10 — good depth | 7/10 — chunking/embedding confusion |
| Fraud detection redesign | N/A | 4/10 — process not system design |
| Kafka in C360 | N/A | 2/10 — circular, no reasoning |
| Budget management | 7.5/10 — honest, structured | N/A |
| Vendor accountability | 9/10 — very specific | N/A |
| Questions asked | 8/10 — good questions | 1/10 — "no questions" |
| **Estimated overall** | **8/10** | **4.5/10** |

---

## THE FOUR THINGS THAT NEED TO FIX BEFORE ANY NEXT ROUND

**1. Kafka — memorize the correct answer.**
"Streaming over batch because: near-real-time freshness, fan-out to multiple independent consumers, replay capability on failure." Three reasons. Thirty seconds. This answer should be a reflex by now.

**2. Fraud detection system design — have an architecture answer.**
When asked to design a system, give layers: data ingestion → feature engineering → model scoring pipeline → real-time decision layer → feedback loop. Even if you have not built it recently, you can reason through it from your BoA experience.

**3. AI measurement — stop saying "there is no way to calculate it."**
There is always a way. Track rework rate, first-submission pass rate, and escaped defect rate. Qualify the method honestly — "manual capture at sprint level" — but do not say it cannot be measured.

**4. Always have 2 questions prepared.** No exceptions.

---

## ONE HONEST OBSERVATION

Round 1 ended with Swetha saying "the recruiter will reach out, and I will have one of my reports also talk to you today in the next hour or so." That is a strong signal from Round 1. Round 2 is where the doubt, if any, was introduced. The Kafka answer in particular is a visible gap for a role explicitly built around data pipelines.

If you get a Round 3, the Kafka answer and the system design depth are the two things most likely to come back up.

---

*Feedback based strictly on WellsData1.txt, WellsData1-1.txt, WellsData2.txt — June 2026*
