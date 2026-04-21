# Section 5: Production & Reliability

## Q1: How do you ensure logging is sufficient?

- I ensure logging through standardization, enforcement, and validation.

- First, I define logging standards—every API must log request, response, errors, and latency.

- Then I enforce this through CI/CD using static analysis and code reviews, so missing logs can block builds.

- More importantly, I validate logging coverage by comparing system transactions with actual logs to ensure there are no gaps.

- Finally, I use dashboards and alerts to detect anomalies like missing logs or mismatched counts.

- So instead of assuming logs exist, I systematically validate observability.


---

## Q6: Tell me about a recent production incident
- Recently, we had an issue where data was not available in OpenSearch due to a failure in index creation during a cluster issue.

- It was detected through monitoring alerts, and we quickly initiated a war room and assessed impact—it affected a specific customer workflow.

- As a mitigation, we reprocessed the data pipeline to restore consistency and ensure APIs could function.

- We kept stakeholders informed throughout the process.

- Post-resolution, we did an RCA and implemented improvements like better dependency validation, stronger alerting, and retry mechanisms.

- We also reviewed similar pipelines to prevent recurrence

## Q7: How do you handle incidents as a manager?

- I handle incidents in four phases—detect, respond, stabilize, and prevent.

- First, incidents are detected via alerts or SLO breaches.

- I coordinate response through a war room and ensure clear ownership while keeping stakeholders informed.

- We prioritize stabilizing the system—through rollback, retries, or graceful degradation.

- After resolution, I ensure RCA is done and preventive actions are implemented.

- My focus is always minimizing customer impact first, then improving system resilience.”

## Q1: How do you ensure production stability
- Monitoring - Track system health through dashboards and alerts  
- Proactive Checks - Review API health, ETL pipelines, and system metrics regularly  
- Risk Prevention - Address vulnerabilities, upgrades, and known risks proactively  
- Feedback Loop - Use production incidents and trends to improve system  
- Outcome - Prevent issues before they impact customers  

---

## Q2: How do you handle production incidents / service down
- Detection - Identify incidents via alerts or SLO breach  
- Response - Activate on-call team and initiate war room  
- Stabilization - Rollback recent changes or apply mitigations  
- Resolution - Fix root cause and validate system stability  
- Improvement - Conduct RCA and strengthen monitoring and resilience  
- Outcome - Minimize customer impact and ensure long-term stability  

---

## Q3: How do you manage support, on-call, and recurring issues
- Ownership - Define clear on-call responsibilities within the team  
- Prioritization - Handle critical issues immediately, others based on priority  
- Tracking - Monitor incidents like API failures, ETL issues, and client SRs  
- Pattern Analysis - Identify recurring issues through trends  
- Fix - Implement permanent solutions instead of temporary patches  
- Outcome - Reduce repeated incidents and improve system stability  

---

## Q4: How do you ensure reliability during changes and releases
- Planning - Align changes with governance and approval processes  
- Validation - Ensure proper testing and readiness before deployment  
- Deployment - Use controlled rollout strategies  
- Monitoring - Track system behavior closely post-release  
- Outcome - Ensure safe and stable production changes  

---

## Q5: How do you manage platform updates and security risks
- Tracking - Monitor platform upgrades, vulnerabilities, and compliance needs  
- Prioritization - Address critical security issues first  
- Coordination - Work with platform and architecture teams  
- Execution - Plan updates with minimal delivery impact  
- Outcome - Maintain system security and compliance  

---

## Q6: How do you drive operational excellence
- Governance - Follow structured release and change management processes  
- Reviews - Participate in ops reviews, audits, and quality checks  
- Metrics - Track incidents, stability, and improvement trends  
- Improvement - Continuously improve based on feedback and learnings  
- Outcome - Ensure high reliability and operational maturity  