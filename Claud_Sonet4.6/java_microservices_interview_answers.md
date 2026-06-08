# Java & Microservices Interview — Questions 0 to 10

---

## Q0. Work done in previous project, role & responsibilities

**Sample Answer:**

In my most recent role at Oracle Cerner, I led the Care Coordination product suite within the Health Data Intelligence platform, which served 120+ healthcare customers and generated $20M+ in annual revenue.

My responsibilities spanned the full engineering lifecycle:
- **Architecture & Design**: Designed microservices-based, event-driven systems on Azure using Spring Boot, Kafka, and Kubernetes.
- **Team Leadership**: Managed a cross-functional team of 15+ engineers (backend, data, QA), running Agile ceremonies and driving delivery cadence.
- **Technical Oversight**: Reviewed designs for distributed data pipelines ingesting clinical data (HL7, FHIR), ensuring scalability and HIPAA compliance.
- **Cloud & DevOps**: Drove the Kubernetes containerisation migration, saving $5M annually in infrastructure costs.
- **Stakeholder Management**: Partnered with product managers, clinical SMEs, and customers to translate requirements into engineering roadmaps.

On the technical side I worked extensively with Spring Boot REST APIs, JPA/Hibernate for data persistence, Kafka for event streaming, Redis for caching, and Azure services (AKS, Service Bus, Blob Storage).

---

## Q1. Spring Boot Service Class — DAO Call, Filter & Group with Java Streams

**Scenario**: Call the DAO to get a list of `Card` objects, filter by Issuer = "Amex", then group by Issuer name.

### Concept

A Spring Boot `@Service` class acts as the business logic layer between the `@Controller` and the `@Repository` (DAO). Java Streams provide a functional, pipeline-style API to process collections in a declarative way.

### Code Example

```java
// Card.java (Entity / DTO)
public class Card {
    private String issuerName;
    private String cardNumber;
    private String cardType;
    // getters, setters
}

// CardRepository.java (DAO)
@Repository
public interface CardRepository extends JpaRepository<Card, Long> {
    List<Card> findAll();
}

// CardService.java (Service)
@Service
public class CardService {

    @Autowired
    private CardRepository cardRepository;

    public Map<String, List<Card>> getAmexCardsGroupedByIssuer() {
        List<Card> allCards = cardRepository.findAll();

        return allCards.stream()
            .filter(card -> "Amex".equalsIgnoreCase(card.getIssuerName()))  // (a) filter Amex
            .collect(Collectors.groupingBy(Card::getIssuerName));           // (b) group by issuer
    }
}
```

### Key Stream Operations Used
| Operation | Purpose |
|---|---|
| `filter()` | Keeps only cards where issuer = "Amex" |
| `collect(Collectors.groupingBy())` | Groups remaining cards into a `Map<String, List<Card>>` |

**Why `equalsIgnoreCase`?** Defensive coding — avoids bugs from data like "AMEX" vs "amex".

---

## Q2. Employees & Departments — One-to-Many JPA Mapping in Event/Entity Classes

### Concept

One Department → Many Employees. JPA maps this with `@OneToMany` on the parent (Department) and `@ManyToOne` on the child (Employee).

### Code Example

```java
// Department.java
@Entity
@Table(name = "departments")
public class Department {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Employee> employees = new ArrayList<>();

    // getters, setters
}

// Employee.java
@Entity
@Table(name = "employees")
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "department_id")  // FK column in employees table
    private Department department;

    // getters, setters
}
```

### Key Points to Mention
- `mappedBy = "department"` tells JPA that `Department` is NOT the owner; `Employee` owns the FK.
- `CascadeType.ALL` means saving/deleting a Department cascades to its Employees.
- `FetchType.LAZY` is preferred to avoid N+1 query problems — employees are loaded only when accessed.
- In Event-driven systems (e.g., Kafka events), you'd typically use DTOs instead of JPA entities directly, mapping them using ModelMapper or MapStruct.

---

## Q3. What is a Native Query in JPA?

### Definition
A **Native Query** is a raw SQL query executed directly against the database, bypassing JPQL (JPA Query Language). You use it when JPQL cannot express the query — e.g., database-specific functions, complex joins, or stored procedure calls.

### Syntax

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {

    // JPQL (entity-based, database-agnostic)
    @Query("SELECT e FROM Employee e WHERE e.department.name = :deptName")
    List<Employee> findByDepartmentJPQL(@Param("deptName") String deptName);

    // Native Query (raw SQL, database-specific)
    @Query(value = "SELECT * FROM employees WHERE department_id = :deptId", nativeQuery = true)
    List<Employee> findByDepartmentNative(@Param("deptId") Long deptId);
}
```

### JPQL vs Native Query

| Aspect | JPQL | Native Query |
|---|---|---|
| Based on | Entity/field names | Table/column names |
| Portability | Database-agnostic | Database-specific |
| Use case | Standard CRUD, associations | Complex SQL, DB functions, performance tuning |
| Risk | None | SQL injection risk if not using `@Param` |

**When I use native queries**: Complex reporting queries with window functions, CTEs, or when the ORM-generated SQL is inefficient and I need to hand-tune it.

---

## Q4. What is JPA Criteria API?

### Definition
The **JPA Criteria API** is a programmatic, type-safe way to build dynamic queries at runtime using Java objects instead of string-based JPQL. It's ideal when query conditions are not known until runtime (e.g., a search form with optional filters).

### Example — Dynamic Search

```java
@Service
public class EmployeeSearchService {

    @PersistenceContext
    private EntityManager entityManager;

    public List<Employee> searchEmployees(String name, String department) {
        CriteriaBuilder cb = entityManager.getCriteriaBuilder();
        CriteriaQuery<Employee> query = cb.createQuery(Employee.class);
        Root<Employee> root = query.from(Employee.class);

        List<Predicate> predicates = new ArrayList<>();

        if (name != null && !name.isEmpty()) {
            predicates.add(cb.like(root.get("name"), "%" + name + "%"));
        }
        if (department != null && !department.isEmpty()) {
            predicates.add(cb.equal(root.join("department").get("name"), department));
        }

        query.where(predicates.toArray(new Predicate[0]));
        return entityManager.createQuery(query).getResultList();
    }
}
```

### Criteria API vs JPQL

| Aspect | Criteria API | JPQL |
|---|---|---|
| Query construction | Programmatic (Java objects) | String-based |
| Type safety | Compile-time checked | Runtime errors |
| Dynamic queries | Excellent | Cumbersome (string concatenation) |
| Readability | Verbose | Concise |

**Real-world use**: In healthcare, patient search forms often have 10+ optional filters — Criteria API handles this cleanly without messy string concatenation.

---

## Q5. Why Should We Go for Stateless Microservices?

### What "Stateless" Means
A stateless microservice **does not store any session or request state between calls**. Each request is self-contained — all needed data comes in the request payload or is fetched from a shared external store (DB, cache).

### Reasons to Choose Stateless

**1. Horizontal Scalability**
Any instance can handle any request. You can add/remove pods (Kubernetes) freely without session affinity, enabling true auto-scaling.

**2. High Availability & Resilience**
If an instance crashes, no session data is lost. Load balancers can route to surviving instances transparently.

**3. Simpler Deployment & Rolling Updates**
No need to drain sessions before replacing a pod. Zero-downtime deployments become trivial.

**4. Better Load Distribution**
No "sticky sessions" needed — load balancers can distribute requests evenly, improving resource utilisation.

**5. Easier Testing**
Stateless services are pure functions — same input always produces same output, making unit and integration testing straightforward.

### How State Is Handled When Needed
- **User sessions** → JWT tokens (client holds state) or distributed cache (Redis)
- **Business state** → persisted to database
- **Distributed workflow state** → Saga pattern or state machines (e.g., AWS Step Functions, Temporal)

**Real-world example**: In my Care Coordination platform, all microservices were stateless. During peak data ingestion periods, we scaled from 3 to 15 pods in under 2 minutes with zero session loss.

---

## Q6. Explain Kafka Architecture & Use Cases

### Kafka Architecture

```
Producers → [Kafka Cluster] → Consumers
               |
          [ZooKeeper / KRaft]  (cluster coordination)
```

**Core Components:**

| Component | Role |
|---|---|
| **Producer** | Publishes messages to a Topic |
| **Topic** | Logical channel for messages; divided into Partitions |
| **Partition** | Ordered, immutable log; unit of parallelism |
| **Offset** | Unique ID per message within a partition |
| **Broker** | Kafka server node storing partitions |
| **Consumer** | Reads messages; tracks its own offset |
| **Consumer Group** | Multiple consumers sharing partition load |
| **Replication** | Each partition replicated across N brokers for fault tolerance |

**Message Flow:**
1. Producer sends a message with a key → Kafka hashes the key to decide which partition.
2. Message appended to that partition's log.
3. Consumers in a group each read from assigned partitions (1 consumer per partition max).
4. Consumers commit offsets — they control "where I've read up to."

### Key Properties
- **At-least-once / Exactly-once** delivery guarantees (configurable)
- **Retention**: Messages stored for configurable time (e.g., 7 days), regardless of consumption
- **High throughput**: Millions of messages/sec via sequential disk I/O and batching

### Use Cases

| Use Case | Example |
|---|---|
| Event streaming | User activity tracking, clickstream analytics |
| Async microservice communication | Order placed → inventory, payment, notification services |
| Log aggregation | Centralise logs from 100s of services |
| Change Data Capture (CDC) | Debezium streams DB changes to downstream systems |
| Data pipelines | Real-time ETL from operational DB to data lake |
| Healthcare | HL7/FHIR event streaming between EHR systems |

**In my experience**: Used Kafka extensively at Oracle Cerner to stream clinical data events (ADT, lab results) between Care Coordination microservices — decoupling producers from consumers and enabling replay of missed events during downtime.

---

## Q7. How Do You Handle Distributed Transactions in Microservices?

### The Problem
In a monolith, a database transaction is atomic (ACID). In microservices, each service has its own DB — a single business operation spanning multiple services cannot use a traditional ACID transaction.

### Approaches

#### 1. SAGA Pattern (Most Common)
A sequence of local transactions, each publishing an event/message to trigger the next step. If a step fails, compensating transactions roll back previous steps.

**Two Flavours:**

**Choreography-based SAGA** (event-driven):
```
Order Service → [OrderCreated event] → Inventory Service → [StockReserved event] 
→ Payment Service → [PaymentProcessed event] → Order marked CONFIRMED
If Payment fails → [PaymentFailed event] → Inventory Service releases stock → Order CANCELLED
```

**Orchestration-based SAGA** (central coordinator):
```
Order Orchestrator calls:
  1. Inventory Service → reserve stock
  2. Payment Service → charge customer
  3. Notification Service → send confirmation
If step 2 fails → Orchestrator calls Inventory Service to release stock
```

| Aspect | Choreography | Orchestration |
|---|---|---|
| Coupling | Loose | Central orchestrator knows the flow |
| Observability | Harder to trace | Easier to monitor |
| Tools | Kafka events | Temporal, AWS Step Functions, Camunda |

#### 2. Two-Phase Commit (2PC)
Distributed protocol with a coordinator that first asks all participants to "prepare" then issues "commit/rollback." Rarely used in microservices — too slow, tight coupling, single point of failure.

#### 3. Outbox Pattern
Service writes to its DB and an "outbox" table in the same local transaction. A separate relay process publishes the outbox events to Kafka — guarantees at-least-once delivery without distributed locking.

#### 4. Eventual Consistency + Idempotency
Accept that services will be temporarily inconsistent. Design all consumers to be idempotent (same message processed twice = same result) and handle compensations gracefully.

**My approach**: I prefer **Orchestration-based SAGA** for complex workflows (clear audit trail, easy to handle failures) and the **Outbox Pattern** to ensure reliable event publishing.

---

## Q8. Java 17 Features

Java 17 is a Long-Term Support (LTS) release. Key features:

### 1. Sealed Classes (JEP 409)
Restrict which classes can extend/implement a class or interface.
```java
public sealed interface Shape permits Circle, Rectangle, Triangle {}
public final class Circle implements Shape { ... }
public final class Rectangle implements Shape { ... }
```
**Use**: Better domain modelling; exhaustive `switch` expressions; prevents unintended subclassing.

### 2. Pattern Matching for `instanceof` (JEP 394)
```java
// Old way
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.length());
}

// Java 16+ / 17
if (obj instanceof String s) {
    System.out.println(s.length());  // s is auto-cast
}
```

### 3. Records (JEP 395 — finalised in Java 16, stable in 17)
Immutable data carriers with auto-generated constructor, getters, `equals`, `hashCode`, `toString`.
```java
public record Point(int x, int y) {}
Point p = new Point(3, 4);
System.out.println(p.x()); // 3
```
**Use**: DTOs, value objects, Kafka event payloads.

### 4. Text Blocks (JEP 378)
Multi-line string literals.
```java
String json = """
    {
        "name": "Sekhar",
        "role": "Engineering Manager"
    }
    """;
```

### 5. Switch Expressions (JEP 361)
```java
String result = switch (day) {
    case MONDAY, TUESDAY -> "Weekday";
    case SATURDAY, SUNDAY -> "Weekend";
    default -> "Unknown";
};
```

### 6. Strong Encapsulation of JDK Internals (JEP 403)
Internal APIs (e.g., `sun.misc.Unsafe`) are strongly encapsulated — forces proper API usage. Important for security.

### 7. `RandomGenerator` API (JEP 356)
Unified, extensible API for random number generation with multiple algorithm choices.

### Summary Table

| Feature | Practical Benefit |
|---|---|
| Sealed Classes | Safer type hierarchies, exhaustive matching |
| Pattern Matching | Less boilerplate casting |
| Records | Compact immutable DTOs |
| Text Blocks | Cleaner SQL/JSON/HTML strings |
| Switch Expressions | Concise, expression-style branching |

---

## Q9. Service Registry & Discovery

### Problem
In microservices, service instances are dynamic — IPs change when pods restart, scale up/down. Services cannot hardcode each other's addresses.

### Solution: Service Registry
A centralised directory where services register their location (host:port) and others look them up by logical name.

### How It Works

```
Service A starts → registers with Registry (name="service-a", host="10.0.1.5", port=8080)
Service B wants to call Service A:
  1. Queries Registry: "Where is service-a?"
  2. Registry returns: "10.0.1.5:8080" (or a list if multiple instances)
  3. Service B calls that address
```

### Discovery Patterns

**Client-Side Discovery** (e.g., Netflix Eureka + Ribbon):
- Client queries registry and picks an instance (client does load balancing).
- More control, but every client needs discovery logic.

**Server-Side Discovery** (e.g., Kubernetes Service + kube-proxy, AWS ALB):
- Client calls a stable DNS name → load balancer queries registry and routes.
- Simpler clients; infrastructure handles routing.

### Popular Tools

| Tool | Type | Notes |
|---|---|---|
| **Netflix Eureka** | Client-side | Classic Spring Cloud choice |
| **Consul** | Both | Health checks, KV store, DNS |
| **Kubernetes DNS** | Server-side | Built-in; ClusterIP services |
| **AWS Cloud Map** | Server-side | AWS-native service discovery |
| **Zookeeper** | Both | Kafka uses it; more complex |

### Spring Cloud Eureka Example
```java
// Eureka Server
@SpringBootApplication
@EnableEurekaServer
public class RegistryApplication { ... }

// Service Client
@SpringBootApplication
@EnableEurekaClient
public class OrderService { ... }

// Calling another service (with load balancing)
@LoadBalanced
@Bean
public RestTemplate restTemplate() { return new RestTemplate(); }

// Usage — "payment-service" is the logical name, not a hardcoded URL
restTemplate.getForObject("http://payment-service/api/pay", String.class);
```

**In Kubernetes**: You don't need Eureka — Kubernetes DNS (`payment-service.default.svc.cluster.local`) and Service objects handle discovery natively. I migrated from Eureka to native K8s discovery at Cerner, which eliminated the registry as a single point of failure.

---

## Q10. Fault Tolerance & Circuit Breaker

### The Problem
In a microservices chain, if Service C is slow or down, Service B's threads pile up waiting for it, eventually causing Service B to fail too. This is **cascading failure**.

### Circuit Breaker Pattern
Inspired by electrical circuit breakers — automatically "trips" to stop calls to a failing service, giving it time to recover.

### Three States

```
CLOSED (normal) → too many failures → OPEN (blocking) → after timeout → HALF-OPEN (testing)
                                                          ↑                    |
                                                          |    success → CLOSED |
                                                          |    failure → OPEN ←-|
```

| State | Behaviour |
|---|---|
| **CLOSED** | Requests flow normally; failures counted |
| **OPEN** | All requests fail fast (fallback returned immediately); no calls to downstream |
| **HALF-OPEN** | Limited test requests allowed; if they succeed, circuit closes |

### Implementation with Resilience4j (Spring Boot)

```java
// pom.xml dependency
// io.github.resilience4j:resilience4j-spring-boot2

// application.yml config
resilience4j:
  circuitbreaker:
    instances:
      paymentService:
        slidingWindowSize: 10          # last 10 calls
        failureRateThreshold: 50       # open if 50%+ fail
        waitDurationInOpenState: 10s   # wait before half-open
        permittedNumberOfCallsInHalfOpenState: 3

// Service class
@Service
public class OrderService {

    @CircuitBreaker(name = "paymentService", fallbackMethod = "paymentFallback")
    public String processPayment(Order order) {
        return paymentClient.charge(order);  // calls payment microservice
    }

    // Fallback — called when circuit is OPEN or call fails
    public String paymentFallback(Order order, Exception ex) {
        return "Payment service temporarily unavailable. Order queued for retry.";
    }
}
```

### Other Fault Tolerance Patterns (Resilience4j)

| Pattern | Purpose | Annotation |
|---|---|---|
| **Retry** | Auto-retry transient failures | `@Retry` |
| **Rate Limiter** | Limit calls per time window | `@RateLimiter` |
| **Bulkhead** | Limit concurrent calls to isolate failures | `@Bulkhead` |
| **Time Limiter** | Timeout on slow calls | `@TimeLimiter` |

### Hystrix vs Resilience4j
Netflix Hystrix was the original standard but is now in **maintenance mode**. **Resilience4j** is the current standard — lightweight, functional, integrates natively with Spring Boot Actuator for metrics and Micrometer/Prometheus monitoring.

**Real-world**: In my Care Coordination platform, we wrapped all external EHR API calls with circuit breakers — when a hospital's FHIR endpoint was slow, our services returned cached data gracefully instead of timing out for end users.

---

*Prepared for Senior Engineering Manager / Director interview preparation — Java & Microservices track*
