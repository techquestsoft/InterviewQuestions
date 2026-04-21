# Section 6: System Design & Architecture
## Q1: How do you scale a system from 120 to 1200 customers?
- I approach scaling in four areas: capacity, architecture, operations, and team scaling.

- First, I baseline current system metrics like throughput, P95 latency, and storage growth, and project 10x load with buffer to estimate infrastructure and cost.

- From an architecture perspective, I ensure services are stateless and horizontally scalable, and for heavy workloads like patient data processing, we use distributed pipelines like EMR and partition data by tenant.

- Operationally, I define SLOs, strengthen observability using metrics, logs, and alerts, and improve on-call readiness.

- From a team perspective, I scale from 2 teams to domain-based pods with clear ownership and improve onboarding and standards.

- Finally, I scale incrementally, onboarding customers in phases to ensure stability.”
---

## Q1: How do you approach system design
- Understanding - Start with business goals and non-functional requirements like scale, availability, and latency  
- Decomposition - Break system into API, processing, and data layers  
- Decisions - Evaluate trade-offs across scalability, maintainability, and ownership  
- Collaboration - Align with architecture and platform teams for standards  
- Outcome - Ensure simple and scalable design aligned with business needs  

---

## Q2: Monolith vs Microservices
- Decision Factors - Evaluate domain complexity, team structure, change frequency, and maturity  
- Monolith - Suitable for simple domains with strong consistency needs  
- Microservices - Enable independent scaling, deployment, and ownership  
- Trade-offs - Add distributed complexity, latency, and operational overhead  
- Approach - Start simple and evolve as system grows  

---

## Q3: How do you design for scalability
- Service Design - Build stateless services for horizontal scaling  
- Load Handling - Distribute traffic to avoid bottlenecks  
- Data Strategy - Optimize data access patterns for scale  
- Async Processing - Use event-driven approach for heavy workloads  
- Outcome - Handle peak load without performance impact  

---

## Q4: Design a notification system
- Architecture - Use event-driven design for decoupling  
- Flow - Producer publishes events and consumer processes them  
- Delivery - Support channels like email and SMS  
- Reliability - Use retries and DLQ for failure handling  
- Tracking - Maintain status and expose via API/webhook  
- Scalability - Partition events for load distribution  

---

## Q5: When do you use Saga (Choreography vs Orchestration)
- Need - Use when transactions span multiple services  
- Choreography - Event-driven and simple but harder to debug  
- Orchestration - Central control for complex workflows and better visibility  
- Trade-off - Simplicity vs control  
- Approach - Prefer orchestration for critical flows  

---

## Q6: How do you design for failure and reliability
- Design - Assume failures across services, network, and dependencies  
- Isolation - Prevent cascading failures across components  
- Recovery - Use retries and fallback mechanisms  
- Degradation - Support partial functionality instead of full outage  
- Monitoring - Detect issues early and respond quickly  
- Outcome - Maintain stability and minimize customer impact  

---

## Q7: How do you design for high availability
- Redundancy - Deploy services across multiple instances  
- Failover - Ensure backup mechanisms for critical components  
- Load Distribution - Avoid single points of failure  
- Monitoring - Use health checks and alerts  
- Outcome - Ensure system availability during failures  

---

## Q8: How do you handle data consistency
- Strong Consistency - Use when correctness is critical  
- Eventual Consistency - Use for scalability in distributed systems  
- Trade-off - Balance consistency vs performance  
- Approach - Prefer eventual consistency with safeguards  
- Outcome - Ensure scalable and reliable data handling  

---

## Q9: Real example from your experience
- Problem - High infrastructure cost and inefficiencies in microservices platform  
- Analysis - Identified underutilized services and inefficiencies  
- Execution - Migrated to more efficient platform  
- Result - Reduced cost by ~ $5M annually and improved scalability  
- Learning - Importance of right platform and resource optimization  