# System Design & Architecture (Unified Format)

---

## Q1: How do you approach system design

**Memory Trick**  
Problem → Break → Trade-offs → Align → Outcome  

### Core Answer
- **Problem** – Start with business goals and NFRs (scale, latency, availability)  
- **Breakdown** – Decompose into API, processing, and data layers  
- **Trade-offs** – Evaluate scalability, maintainability, and ownership  
- **Alignment** – Align with platform and architecture standards  
- **Outcome** – Design simple, scalable systems  

### If Probed
- Focus on **business-first design, not tech-first**  
- Clearly articulate **trade-offs and decisions**  

---

## Q2: Monolith vs Microservices

**Memory Trick**  
Simple → Monolith | Scale → Microservices  

### Core Answer
- Use **monolith** for simpler systems with strong consistency needs  
- Use **microservices** for independent scaling and team ownership  
- Decision based on domain complexity and change frequency  

### If Probed
- Highlight trade-offs:
  - Microservices → scalability + ownership  
  - Cost → complexity, latency, ops overhead  
- Prefer **evolution approach (start simple → scale)**  

---

## Q3: How do you design for scalability

**Memory Trick**  
Stateless → Distribute → Optimize → Async  

### Core Answer
- Design stateless services for horizontal scaling  
- Distribute load effectively  
- Optimize data access  
- Use async processing for heavy workloads  

### If Probed
- Use **event-driven architecture (Kafka, queues)**  
- Focus on **bottleneck identification and removal**  

---

## Q4: How do you scale system from 120 → 1200 customers

**Memory Trick**  
Capacity → Architecture → Ops → Team → Execution  

### Core Answer
- **Capacity** – Baseline metrics (throughput, latency, storage)  
- **Architecture** – Stateless services, partition data by tenant  
- **Operations** – Define SLOs, improve observability  
- **Team** – Scale to domain-based ownership  
- **Execution** – Incremental onboarding  

### If Probed
- Plan for **10x growth with buffer**  
- Balance **tech scaling + team scaling**  

---

## Q5: What changes beyond architecture when system scales

**Memory Trick**  
Scale = Tech + Quality + Process  

### Core Answer
- Scaling involves **architecture, quality, and process**  
- Introduce release gates and standards  
- Improve observability and operations  

### If Probed
- Focus on **engineering maturity and consistency across teams**  

---

## Q6: Kafka fails after DB commit

**Memory Trick**  
DB ✅ Event ❌ → Outbox  

### Core Answer
- Problem: DB success but event failure → inconsistency  
- Solution: Use **Outbox pattern**  
- Persist event with business data  
- Retry publishing  

### If Probed
- Ensure **eventual consistency + no data loss**  

---

## Q7: Payment service failure

**Memory Trick**  
Fail Fast → Retry → Protect → Compensate  

### Core Answer
- Fail fast to avoid blocking  
- Use retries and circuit breakers  
- Provide user retry options  
- Use Saga for compensation  

### If Probed
- Use DLQ for failed events  
- Maintain **user experience + system stability**  

---

## Q8: Testing doesn’t scale

**Memory Trick**  
Manual ❌ → Automation + Priority  

### Core Answer
- Shift from manual to automation  
- Focus on regression and critical paths  
- Improve test data management  

### If Probed
- Optimize for **speed + coverage balance**  

---

## Q9: Design a Notification System

**Memory Trick**  
Producer → Queue → Service → Channel → Retry  

### Core Answer
- Event-driven design  
- Producers publish to queue  
- Service processes and sends notifications  
- Support multiple channels (email, SMS, push)  
- Use retries and DLQ  

### If Probed
- Ensure **idempotency and delivery guarantees**  
- Partition for scalability  

---

## Q10: When do you use Saga

**Memory Trick**  
Choreo → Simple | Orchestrator → Control  

### Core Answer
- Use Saga for distributed transactions  
- Choreography for simple flows  
- Orchestration for complex workflows  

### If Probed
- Prefer orchestration for **critical systems (better control + observability)**  

---

## Q11: How do you design for failure

**Memory Trick**  
Fail → Isolate → Recover → Degrade  

### Core Answer
- Assume failures will happen  
- Isolate failures  
- Implement retries and fallback  
- Support graceful degradation  

### If Probed
- Detect early using **observability and alerts**  

---

## Q12: How do you design for high availability

**Memory Trick**  
No Single Point of Failure  

### Core Answer
- Use redundancy and multiple instances  
- Implement failover  
- Distribute load  

### If Probed
- Multi-region and disaster recovery  

---

## Q13: How do you handle data consistency

**Memory Trick**  
Correctness vs Scale  

### Core Answer
- Use strong consistency when correctness is critical  
- Use eventual consistency for scalability  

### If Probed
- Balance **consistency vs performance trade-offs**  

---

## Q14: Real example

**Memory Trick**  
Analyze → Optimize → Save  

### Core Answer
- Identified inefficiencies in platform  
- Optimized services and infrastructure  
- Reduced cost significantly  

### If Probed
- Demonstrates **platform thinking and ROI focus**  

---

## Q15: 12 Factor App

### Core Answer
- Used for cloud-native scalability and maintainability  
- Stateless services  
- Externalized config  
- Logs as event streams  

### If Probed
- Ensures **consistency across environments and teams**  

---
