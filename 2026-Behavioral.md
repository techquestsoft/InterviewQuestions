# Section 9: Behavioral (Strengths / Failures)

## Q1: What are your key strengths
- Ownership - Take end-to-end ownership from planning to production stability  
- Problem Solving - Handle production issues calmly using structured approach  
- Team Leadership - Build strong teams through clear expectations and mentoring  
- Technical Judgment - Make balanced decisions across scalability, reliability, performance, cost, and delivery timelines
- Business Alignment - Align engineering work with business and customer impact  

---

## Q2: What are your weaknesses
- Context Switching - Earlier, I used to respond to problems from a deep technical lens, even in leadership scenarios; I’ve improved by consciously separating system design vs operational/management responses

- Structured Communication - In some cases, I tend to go deep into details instead of leading with a crisp, structured summary; I’m actively improving by focusing on “answer first, details later”

- Delegation Depth - I used to take on critical problem-solving myself; I’m improving by pushing ownership to senior engineers and focusing more on direction and decision-making

---

## Q3: Tell me about a failure
- Situation - Underestimated complexity during platform migration  
- Impact - Caused delays in delivery timelines  
- Action - Reworked plan, improved estimation, aligned stakeholders  
- Learning - Validate assumptions early and plan dependencies better  
- Outcome - Improved planning discipline in future initiatives  

During a platform migration from OpenShift to Kubernetes, we initially underestimated the overall scope and complexity.

As we progressed, we identified several redundant services and also saw an opportunity to build common components instead of duplicating functionality. While this was the right long-term approach, it increased the scope and impacted our timelines.

Instead of continuing with the original plan, I took a step back and discussed this with product and leadership. I presented the trade-offs—short-term delay versus long-term benefits like better maintainability and reduced infrastructure cost.

We aligned to proceed with the optimized approach, and eventually it helped us reduce costs significantly and improve platform efficiency.

The key learning for me was to validate assumptions earlier and proactively look for platform-level optimizations during the estimation phase itself.

## Follow-up 1: “Why didn’t you identify this earlier?”
That’s a fair point. In the initial phase, our focus was primarily on migration and timelines, and we didn’t do a deep enough analysis of service usage and duplication.

As we got into execution, the gaps became more visible, especially around redundant services and repeated logic.

Since then, I’ve improved my approach by adding an explicit discovery and assessment phase before large migrations, where we evaluate dependencies, duplication, and optimization opportunities upfront.

This has helped improve estimation accuracy and avoid surprises in later phases.

## Follow-up 2: “How did you convince stakeholders to accept delays?”
I focused on framing the discussion in terms of business impact rather than just technical improvements.

I presented two options:

Continue with the current plan and meet timelines but carry forward inefficiencies
Or invest additional time to remove redundancy, build common components, and reduce long-term cost and maintenance effort

I quantified the benefits in terms of reduced infrastructure cost and improved maintainability, and also explained the risks of not addressing it now.

That helped stakeholders align on taking a short-term delay for a better long-term outcome.

## Follow-up 3: “What would you do differently next time?”
I would introduce a structured pre-migration assessment phase as part of planning.

This would include:

Service usage analysis
Identifying redundant or low-value services
Evaluating opportunities for shared components

I would also involve architecture and product teams earlier in these discussions to align on trade-offs upfront.

This ensures better estimation accuracy and avoids mid-execution scope changes.


---

## Q4: How do you handle pressure situations
- Structure - Break problems into smaller manageable parts  
- Prioritization - Focus on critical issues first  
- Communication - Keep stakeholders and team aligned  
- Execution - Address immediate issues and plan long-term fixes  
- Outcome - Maintain control and guide team effectively under pressure  

---

## Q5: How do you handle conflicts (team or stakeholders)
- Understanding - Listen and understand all perspectives  
- Alignment - Align discussions to business goals  
- Decision - Evaluate trade-offs like impact, timeline, and risk  
- Resolution - Drive balanced or phased solutions  
- Example - Balanced model upgrade vs observability priorities  

---

## Q6: How do you motivate and manage difficult team members
- Understanding - Identify root cause (skill, clarity, or motivation)  
- Feedback - Provide clear and constructive feedback  
- Support - Offer guidance and structured improvement plan  
- Ownership - Encourage accountability and involvement  
- Outcome - Most team members improve with right support  

---

## Q7: What is your leadership style
- Approach - Collaborative and outcome-driven  
- Decision Making - Involve team but take final accountability  
- Focus - Balance delivery, quality, and team growth  
- Outcome - Build trust and consistent team performance  