# Evernorth AD Final Round — Q&A in Your Voice
### Leadership · Strategy · Vision · Commercial Acumen
### Rajasekhar Reddy Yakkaluru (Sekhar)

> Built from the AD Interview Master Guide + your real experience. AI kept deliberately light — used only where it's genuinely your story (the readmission agent, AI-assisted dev), never as filler. Every answer is grounded in things you've actually done: Cerner Care Coordination, Optum K8s migration, BoA fraud, Kafka/OpenSearch data platforms.

---

## THE THREE PILLARS TO TRACE EVERY ANSWER BACK TO

The guide is right: at AD level, every answer should ladder up to one of these. Keep them in your head.

1. **Risk & Compliance first** — in healthcare, compliance trumps raw speed. HIPAA/HITRUST/PHI discipline is non-negotiable.
2. **Value creation over cost arbitrage** — move the India team from "order-takers" to owners of complete product lifecycles.
3. **Commercial acumen** — speak P&L: margins, resource pyramid, tech-debt cost, retention, time-to-market.

The shift from your earlier rounds: **from "execution and delivery" → to "strategy, scale, and organizational risk."** That's the altitude for this panel.

---

# PART 1: REGULATORY / RISK & CRISIS

### Q1. Tell me about a delivery crisis you handled under regulatory or compliance pressure.

**What they're testing:** Do you put compliance above speed? Regulatory maturity.

**Framework:** Isolate → compliant mitigation → root-cause → systemic control so it can't recur.

**Your answer:**
> "In my Care Coordination team at Oracle Cerner, we had a sprint with two compliance-driven deliverables running together — a security vulnerability fix and a mandatory upgrade. Mid-sprint, I caught through our burn-down review that we were going off track, and the temptation on the team was to rush the security fix in to hit the date.
>
> I made the call to slow down rather than push a rushed fix. In healthcare, getting a security or compliance item wrong is far costlier than slipping a sprint. So I moved the vulnerability fix to the next sprint with proper scope, kept the compliance upgrade on track, and — the important part — I changed our process so dependencies and compliance checks are mapped during grooming, before work is pulled into a sprint, not discovered mid-flight.
>
> We delivered the compliance work on time, and the process change reduced spillovers across the team in the following sprints. The lesson I carry is simple: under regulatory pressure, speed without control is a false economy."

**Why it lands:** real story, compliance-over-speed judgment, systemic fix at the end. Pure altitude.

---

### Q2. Have you had to halt or roll back something for a compliance/security reason against pressure to ship?

**What they're testing:** spine under pressure; will you protect the patient/client over the deadline.

**Your answer:**
> "Yes. The principle I hold is that in healthcare IT, when there's a credible compliance or PHI-exposure risk, the deployment stops — full stop — regardless of the deadline pressure. I'd rather take the short-term hit of a rollback and a transparent incident log than risk patient data or client trust.
>
> What I focus on after the immediate containment is the systemic side — getting the guardrail into the pipeline so the same class of issue can't reach production again. For example, building checks into the CI/CD gates so a problem is caught before staging, not after release. Containing one incident is table stakes; making sure it can't recur is the actual job.
>
> Clients don't lose trust because you had an issue — every system has issues. They lose trust when you hide it or repeat it. Transparency plus a permanent fix is what preserves the relationship."

**Note:** this is partly principle, partly approach — fine for this one, since it's about judgment. If they want a specific incident, pivot to the Q1 story.

---

# PART 2: STAKEHOLDERS, TECH DEBT & MODERNIZATION

### Q3. Tell me about managing a client/stakeholder escalation tied to legacy systems and technical debt.

**What they're testing:** stakeholder negotiation; balancing modernization with stability; tying tech to ROI.

**Framework:** Validate the pain → tie modernization to an ROI/risk number → propose a ring-fenced, phased plan.

**Your answer:**
> "On the product side, the recurring tension is the product team pushing client features while engineering carries tech debt, security, and compliance work. In one case the pressure was to deliver client features faster, and the instinct from above was to just add hours.
>
> Instead of throwing hours at it, I made the cost visible. I showed how much of our capacity was being consumed maintaining legacy integrations rather than building new value. Then I proposed a ring-fenced approach — carve out a fixed slice of the team to decouple the core into a cleaner, API-based services layer in phases, while the rest of the team kept delivering the highest-priority client features. I used an 80/20 lens: identify the 20% of features delivering 80% of the client value, ship those first, phase the rest.
>
> The key was reserving a steady percentage of every sprint for tech debt and compliance, so the system stays stable while we modernize. We kept the client moving and made the delivery sustainable instead of a weekend death-march. Transparency about trade-offs is what turns an escalation into a partnership."

**Your governance hook:** mention your **Change Control Board (CCB) / feature review** as the forum where these trade-offs get decided transparently. It shows you run this with real process, not ad hoc.

---

### Q4. How do you decide what to modernize and what to leave alone?

**What they're testing:** judgment; you don't modernize for its own sake; commercial thinking.

**Your answer:**
> "I don't modernize for the sake of it — I modernize where there's a business case: cost, risk, or velocity. I look at three things. First, what's the maintenance cost of leaving it — how much capacity is it eating? Second, what's the risk — security, compliance, or stability exposure? Third, what's the velocity drag — is it slowing every new feature?
>
> If a legacy component is stable, low-risk, and not blocking delivery, I leave it alone — that's the disciplined call. Where it's bleeding cost or blocking the roadmap, I make the case with numbers and modernize it in ring-fenced phases with a rollback path, never a big-bang rewrite.
>
> My Optum platform modernization was exactly this — we moved 110+ microservices off the older platform onto Kubernetes because the cost and scaling constraints were real and quantifiable. That delivered about $5M in annual infrastructure savings. The decision was driven by the numbers, not by wanting the newest stack."

---

# PART 3: SCALING THE INDIA CENTER (THE CORE THEME)

### Q5. How do you move the Hyderabad team from a resource/execution hub to a value-creating Center of Excellence?

**What they're testing:** macro leadership vision; the GCC maturity model; global alignment.

**Framework:** Audit current output → build domain depth → shift ownership of modules to India.

**Your answer:**
> "The shift I'd drive is from an 'extension' model — where the US defines architecture and India writes the code — to genuine end-to-end ownership. Three moves.
>
> First, build domain depth. In healthcare, the biggest unlock is when engineers understand the clinical *why*, not just the technical *what*. When my India team at Cerner truly understood care coordination and readmission workflows, they stopped being task-takers and started spotting product gaps the US team hadn't seen.
>
> Second, shift ownership one capability at a time. Pick one module, move full ownership to India — requirements, design, delivery, and support — and prove the model before scaling it. One real proof point beats a slogan.
>
> Third, embed India in the upstream conversations — product planning and architecture — so the team shapes requirements rather than just receiving them.
>
> At Cerner, my India team owned two Care Coordination products end to end — 120+ customers, $20M+ in annual revenue. That's the model: India as an owner with a P&L line, not a cost center."

**This is your central narrative — it ties to all three interviewers and to Anant's published thesis. Lead with it whenever vision comes up.**

---

### Q6. What does your first 30-60-90 days look like?

**What they're testing:** whether you arrive with a plan; whether you think like a partner, not a hire.

**Your answer — say it in three clean phases:**

> "I'd run it in three phases.
>
> **First 30 days — discovery and alignment.** 1:1s with the US leaders, the local managers, and key stakeholders to understand their real pain points and how each defines success. A compliance and delivery-pipeline review to find any latent risk. And an honest assessment of my managers and tech leads — where the leadership bench is strong and where it needs building.
>
> **Days 31-60 — quick win and strategy.** Find one high-friction bottleneck — a slow pipeline, a recurring reporting pain — and fix it, to build trust early. Start shifting the team's metrics from effort — hours worked — to outcomes: SLA compliance, defect leakage, delivery velocity. And bring the leadership a six-month roadmap.
>
> **Days 61-90 — execution and scaling.** Begin moving one domain to full India ownership as the proof point. Shift myself from running daily operations to coaching my managers so they own their delivery and metrics. And put automated dashboards in place so the US leaders get real-time visibility without chasing status reports."

**Tip:** offering this *proactively* — "if it's useful, here's how I'd approach the first 90 days" — is a strong, senior move. It makes you sound like a partner, not a candidate.

---

# PART 4: COMMERCIAL ACUMEN / P&L (NEW — PRACTICE THIS)

This is the dimension your earlier rounds didn't cover. An AD owns the P&L. Get comfortable with this language.

### Q7. How do you manage margins and resource optimization?

**What they're testing:** financial acumen; can you run this profitably.

**Framework:** analyze cost centers → optimize the team pyramid → automate the repetitive.

**Your answer:**
> "Margin pressure in Hyderabad is real with rising talent costs, so I manage it on three levers.
>
> First, the team pyramid. A common problem is a top-heavy structure — senior, expensive engineers doing work that doesn't need their level, like routine L3 support. I rebalance: bring in strong mid-level engineers, give them clear runbooks, and move routine support to that tier — which frees senior talent for the high-value modernization work that actually drives the business.
>
> Second, automation. Anything repetitive — manual deployments, manual reporting, repeated support toil — gets automated so capacity goes to value-adding work, not overhead.
>
> Third, retention. In this market, attrition is a hidden margin killer — every senior exit costs ramp time and knowledge. So keeping good people through meaningful work and growth directly protects margin.
>
> Done together, you improve gross margin and senior-engineer satisfaction at the same time — because your best people spend their time on work that matters."

### Q8. How do you think about the cost of technical debt in commercial terms?

**Your answer:**
> "I translate tech debt into the language leadership cares about — cost and velocity. Debt shows up as capacity spent on maintenance instead of new value, and as slower delivery and more production incidents over time. So I quantify it: what percentage of capacity is maintenance versus new build, and what's that trending toward. Once it's a number, the conversation stops being 'engineers want to refactor' and becomes 'here's the cost of not acting.' That's how you win investment in modernization — by showing the P&L impact, not the technical detail."

---

# PART 5: PEOPLE & LEADERSHIP

### Q9. How do you build and motivate a high-performing team?

**Your answer:**
> "Three things. Hire to a clear bar — I stay personally involved in senior hiring and keep the loop calibrated. Grow leaders, not just engineers — at Cerner I grew two tech leads into managers by handing them real ownership, like running sprint planning and hiring loops. And give people meaningful work — in healthcare, connecting the work to patient impact is a real motivator: the readmission score you're improving helps a care manager reach a patient who needs intervention. On top of that, recognition and protecting people from being stuck on low-value maintenance. My measure of success as a leader is how many of my people get promoted — and how many stay. We held retention above 90% in a hot market."

### Q10. Tell me about your most challenging people situation.

**Your answer (have a real one ready, STAR):**
> Pick a genuine performance or conflict situation. Structure: situation → the gap → what you did (feedback, support, clear expectations, and the decision) → outcome → lesson. Show **empathy and accountability together.** End with what you learned — usually about earlier feedback or sharper hiring. Avoid blaming the person; own your part of the relationship.

### Q11. What kind of manager brings out your best — and which do you struggle with?

**Your answer (self-awareness, no blame):**
> "I do my best with a manager who sets clear context and outcomes, then trusts me on the *how* — autonomy with accountability. I also value directness; I'd rather get honest feedback early than find a problem late. Where I have to work harder is with a very hands-on, detail-level manager who wants to be deep in the day-to-day *how*. What I've learned is that's usually about trust not being built yet — so I over-communicate up front and let results earn the autonomy over time. So even there, I make the relationship work."

---

# PART 6: AI — KEPT LIGHT (only your real story)

Don't lead with AI. Use it only if asked, and keep it grounded and governed — which is the healthcare-correct stance anyway.

### Q12. How do you think about applying AI in healthcare?

**Your answer (pragmatic, risk-first — this is the right tone for healthcare):**
> "I'm deliberately pragmatic about it. In healthcare, the risk of an AI system making an autonomous clinical decision — and getting it wrong — is a serious liability. So I steer toward low-risk, high-impact uses: reducing administrative burden, supporting a human rather than replacing a clinical judgment, always with a person in the loop.
>
> In my own work, we built a conversational assistant for care managers to query patient risk data in plain language — but with strict grounding, so it answers only from our trusted data and says 'I don't have that' rather than guessing. A confident wrong answer in healthcare is worse than no answer.
>
> The other place I use AI is engineering productivity — AI-assisted development to speed up routine coding and testing. That's a safe, immediate win. My overall view: AI is an accelerant, governed carefully — not the strategy itself."

**Keep this to ~30 seconds unless they dig. Then stop.** The strategy is healthcare delivery; AI is one tool.

---

# PART 7: REVERSE QUESTIONS (ASK THESE — pick 2-3)

Strategic questions mark you as AD material. Tailor to who's answering.

**For Mike (VP):**
> "If we're sitting here twelve months from now celebrating a great year, what's the single most important business metric we've moved — growth, margin, or stability?"
*(Forces him to name the real priority; shows you're outcome-oriented.)*

**For Anant (your future manager):**
> "What's the biggest roadblock today preventing the Hyderabad team from taking full global ownership of a product suite?"
*(Speaks directly to the GCC maturity model — his published thesis.)*

**For Christie (App Dev Sr Director):**
> "Healthcare IT is balancing heavy legacy tech debt against pressure to modernize and adopt FHIR interoperability. Where does the team feel that friction most right now?"
*(Proves domain depth; speaks their exact language.)*

**Avoid:** working hours, leave, comp, WFH — any operational question is a red flag at this level.

---

# QUICK REFERENCE — THE NIGHT BEFORE

**Ladder every answer to a pillar:** risk/compliance · value over cost · commercial acumen.

**Altitude check:** business outcomes and org design, not Kafka internals. The risk here is going *too technical*, the opposite of your earlier rounds.

**Two reflexes (from the mock):**
- One breath → play the question back → confirm → answer. (Protects against accent gaps AND answering the wrong question.)
- "Tell me about a time" = one specific STAR story, past tense, with outcome + lesson.

**Land every answer** on a takeaway — don't trail off. (Your one recurring delivery habit.)

**Names, clean and slow:** Michael Long · Christie Rivest · Anant Anand. And your own: Rajasekhar Reddy Yakkaluru — I go by Sekhar.

**Numbers you own:** $5M K8s savings · 110+ microservices · 120+ customers · $20M+ revenue · 500M+ records · 90%+ retention. These are your credibility anchors — use them.
