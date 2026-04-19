
# Section 6: System Design & Architecture

## Q1: How do you approach system design

- Understanding  
  - I start with business goals and key non-functional requirements like scale, availability, and latency  

- Decomposition  
  - I break the system into logical components such as API, processing, and data layers  

- Decisions  
  - I evaluate trade-offs based on scalability, maintainability, and team ownership  

- Collaboration  
  - I align with architecture and platform teams for standards and governance  

- Outcome  
  - Ensure simple, scalable design aligned with business needs  

---

## Q2: Monolith vs Microservices

- Decision Factors  
  - Domain complexity, team structure, change frequency, and operational maturity  

- Monolith  
  - Suitable for simpler domains and strong transactional consistency  

- Microservices  
  - Enables independent scaling, deployments, and team ownership  

- Trade-offs  
  - Adds distributed complexity, latency, and operational overhead  

- Approach  
  - Start simple and evolve to microservices as system grows  

---

## Q3: How do you design for scalability

- Service Design  
  - Build stateless services to enable horizontal scaling  

- Load Handling  
  - Distribute traffic effectively to avoid bottlenecks  

- Data Strategy  
  - Optimize data access patterns for increasing load  

- Async Processing  
  - Use event-driven approach for heavy or background workloads  

- Outcome  
  - System handles peak load without performance impact  

---

## Q4: Design a notification system

- Architecture  
  - Event-driven system for decoupled communication  

- Flow  
  - Producer publishes events → consumer processes and enriches  

- Delivery  
  - Supports multiple channels like email and SMS  

- Reliability  
  - Retry with backoff and handle failures through DLQ  

- Tracking  
  - Maintain status and provide visibility via API/webhook  

- Scalability  
  - Partition events for load distribution  

---

## Q5: When do you use Saga (Choreography vs Orchestration)

- Need  
  - Used when transactions span multiple services  

- Choreography  
  - Simple flows with independent event-driven interactions  
  - Harder to track and debug  

- Orchestration  
  - Complex workflows with centralized control  
  - Better visibility and failure handling  

- Trade-off  
  - Simplicity vs control  

- Approach  
  - Prefer orchestration for critical business flows  

---

## Q6: How do you design for failure and reliability

- Design  
  - Assume failures will happen across services, network, and dependencies  

- Isolation  
  - Prevent cascading failures across components  

- Recovery  
  - Use retries and fallback mechanisms for transient failures  

- Degradation  
  - Support partial functionality instead of full outage  

- Monitoring & Response  
  - Detect issues early and handle through structured incident response  

- Outcome  
  - Maintain system stability and minimize customer impact  

---

## Q7: How do you design for high availability

- Redundancy  
  - Deploy services across multiple instances  

- Failover  
  - Ensure backup mechanisms for critical components  

- Load Distribution  
  - Avoid single points of failure  

- Monitoring  
  - Continuous health checks and alerts  

- Outcome  
  - System remains available during failures or outages  

---

## Q8: How do you handle data consistency

- Strong Consistency  
  - Used when correctness is critical  

- Eventual Consistency  
  - Used in distributed systems for scalability  

- Trade-off  
  - Balance between consistency and performance  

- Approach  
  - Prefer eventual consistency with safeguards for critical flows  

---

## Q9: Real example from your experience

- Problem  
  - High infrastructure cost and inefficiencies in existing microservices platform  

- Approach  
  - Analyzed service usage and identified optimization opportunities  

- Execution  
  - Migrated services to a more efficient platform  

- Result  
  - Reduced cost by ~ $5M annually and improved scalability  

- Learning  
  - Importance of right platform choice and resource optimization  
