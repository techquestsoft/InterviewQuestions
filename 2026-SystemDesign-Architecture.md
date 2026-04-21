# Section 6: System Design & Architecture
## Q1: How do you scale a system from 120 to 1200 customers?
- I approach scaling in four areas: capacity, architecture, operations, and team scaling.

- First, I baseline current system metrics like throughput, P95 latency, and storage growth, and project 10x load with buffer to estimate infrastructure and cost.

- From an architecture perspective, I ensure services are stateless and horizontally scalable, and for heavy workloads like patient data processing, we use distributed pipelines like EMR and partition data by tenant.

- Operationally, I define SLOs, strengthen observability using metrics, logs, and alerts, and improve on-call readiness.

- From a team perspective, I scale from 2 teams to domain-based pods with clear ownership and improve onboarding and standards.

- Finally, I scale incrementally, onboarding customers in phases to ensure stability.”

---

## Q2: What changes beyond architecture when system scales?

- Scaling is not just an architecture problem—it’s also a quality, process, and team maturity problem.

- I introduce release gates to ensure quality doesn’t degrade—for example, enforcing test coverage thresholds, defect leakage limits, and security checks in CI/CD.

- I also strengthen operational readiness by defining SLOs, improving observability, and ensuring strong incident response.

- From a process perspective, I standardize coding, logging, and deployment practices, and improve onboarding for new engineers.

- This ensures the system scales with consistent quality and reliability, not just capacity.”

---

## Q3: What happens if Kafka fails after DB commit?

- This creates inconsistency where data is stored but events are not published.

- To handle this, I use the Outbox pattern, where events are stored in the database along with business data.

- A separate process publishes events to Kafka, ensuring reliability.

- If Kafka is down, events are retried later, ensuring eventual consistency.

- This avoids data loss and ensures system reliability.”

---

## Q4: Payment service failure – how do you handle?

- I handle this by focusing on resilience and user impact.

- First, I fail fast instead of blocking the system.

- Then I use retries with backoff and circuit breakers to avoid cascading failures.

- From a user perspective, I ensure clear feedback and allow retry.

- For data consistency, I use Saga patterns for compensation.

- Additionally, failed events can be pushed to DLQ for recovery.

- The goal is to maintain system stability and user experience.

---

## Q5: Testing doesn’t scale with customers – what do you do?

- When testing doesn’t scale, I shift from manual-heavy testing to automation and risk-based testing.

- I prioritize critical and high-impact scenarios and automate regression suites.

- I also balance speed vs coverage based on business needs.

- Additionally, I improve test data management and reuse across customers.

- This allows faster onboarding without compromising critical quality.

---

## Q6: When do you use Saga?

- I use Saga when transactions span multiple services and cannot be handled with a single ACID transaction.

- For simple flows, choreography works well, but for complex and critical flows, I prefer orchestration for better control and observability. Its event driven.

- In critical systems, especially healthcare workflows, I prefer orchestration to ensure better failure handling and traceability. It works in sequence.

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


## Diagram (Mermaid)

## Diagram (Mermaid)

```mermaid
flowchart TD
    A[Producer] --> B[Message Queue - Kafka]
    B --> C[Notification Service]
    C --> D1[Email]
    C --> D2[SMS]
    C --> D3[Push]
    C --> E[Retry / DLQ]
    C --> F[Status Tracking DB]
    F --> G[API / Webhook]