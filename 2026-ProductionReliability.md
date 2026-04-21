# Section 5: Production & Reliability

---

## Q1: How do you ensure production stability

**Memory Trick**  
Monitor → Prevent → Learn  

- **Monitoring** - I track system health using dashboards, alerts, and key metrics  

- **Proactive Checks** - I regularly review API health, ETL pipelines, and system behavior  

- **Risk Prevention** - I address vulnerabilities, upgrades, and known risks proactively  

- **Feedback Loop** - I use production incidents and trends to continuously improve the system  

- **Outcome** - I prevent issues before they impact customers  

---

## Q2: How do you ensure logging is sufficient

**Memory Trick**  
Standardize → Enforce → Validate  

- **Standardization** - I define logging standards where every API logs request, response, errors, and latency  

- **Enforcement** - I enforce logging through CI/CD using static analysis and code reviews  

- **Validation** - I validate logging coverage by comparing system transactions with actual logs  

- **Monitoring** - I use dashboards and alerts to detect missing logs or anomalies  

- **Outcome** - I ensure observability is complete and reliable, not assumed  

---

## Q3: How do you handle production incidents / service down

**Memory Trick**  
Detect → Respond → Stabilize → Improve  

- **Detection** - I identify incidents through alerts or SLO breaches  

- **Response** - I activate on-call teams, initiate a war room, and assign clear ownership  

- **Stabilization** - I rollback changes or apply mitigations like retries or graceful degradation  

- **Resolution** - I fix the root cause and validate system stability  

- **Improvement** - I conduct RCA and strengthen monitoring and resilience  

- **Outcome** - I minimize customer impact and ensure long-term stability  

---

## Q4: Tell me about a recent production incident

**Memory Trick**  
Detect → Fix → Align → Improve  

- **Situation** - We had an issue where data was not available in OpenSearch due to index creation failure during a cluster issue  

- **Detection** - It was identified through monitoring alerts  

- **Response** - I initiated a war room, assessed impact, and identified affected customer workflows  

- **Mitigation** - We reprocessed the data pipeline to restore consistency and ensure APIs functioned correctly  

- **Communication** - I kept stakeholders informed throughout the incident  

- **Improvement** - Post-resolution, we implemented better dependency validation, stronger alerting, and retry mechanisms  

- **Outcome** - We restored system stability and improved resilience to prevent recurrence  

---

## Q5: How do you manage support, on-call, and recurring issues

**Memory Trick**  
Own → Prioritize → Analyze → Fix  

- **Ownership** - I define clear on-call responsibilities within the team  

- **Prioritization** - I handle critical issues immediately and others based on priority  

- **Tracking** - I track incidents like API failures, ETL issues, and client SRs  

- **Pattern Analysis** - I identify recurring issues through trend analysis  

- **Fix** - I implement permanent fixes instead of temporary patches  

- **Outcome** - I reduce repeated incidents and improve system stability  

---

## Q6: How do you ensure reliability during changes and releases

**Memory Trick**  
Plan → Validate → Deploy → Monitor  

- **Planning** - I align changes with governance and approval processes  

- **Validation** - I ensure proper testing and readiness before deployment  

- **Deployment** - I use controlled rollout strategies  

- **Monitoring** - I track system behavior closely after release  

- **Outcome** - I ensure safe and stable production changes  

---

## Q7: How do you manage platform updates and security risks

**Memory Trick**  
Track → Prioritize → Coordinate → Execute  

- **Tracking** - I monitor platform upgrades, vulnerabilities, and compliance requirements  

- **Prioritization** - I prioritize critical security issues and mandatory updates  

- **Coordination** - I work closely with platform and architecture teams  

- **Execution** - I plan updates to minimize impact on delivery  

- **Outcome** - I maintain system security and compliance  

---

## Q8: How do you handle incidents as a manager

**Memory Trick**  
Detect → Coordinate → Stabilize → Prevent  

- **Detection** - Incidents are identified via alerts or SLO breaches  

- **Coordination** - I lead response through a war room, ensure ownership, and keep stakeholders informed  

- **Stabilization** - I focus on restoring system through rollback, retries, or graceful degradation  

- **Prevention** - I ensure RCA is completed and preventive actions are implemented  

- **Outcome** - I minimize customer impact and improve system resilience  

---

## Q9: How do you drive operational excellence

**Memory Trick**  
Govern → Measure → Improve  

- **Governance** - I follow structured release and change management processes  

- **Reviews** - I participate in ops reviews, audits, and quality checks  

- **Metrics** - I track incidents, stability, and improvement trends  

- **Improvement** - I continuously improve based on feedback and learnings  

- **Outcome** - I ensure high reliability and operational maturity  

---