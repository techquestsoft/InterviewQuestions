# Java, Spring Boot, REST APIs & Microservices
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

---

## Q1: How do you design and operate scalable REST APIs and microservices using Java/Spring Boot?

**Memory Trick:** Contract → Boundary → Resilience → Observability → Secure

- **API contract first** – OpenAPI/Swagger specs reviewed before coding. Versioned APIs (`/v1/`, `/v2/`) so changes don't break consumers.
- **Clear service boundaries** – Each microservice owns a bounded context (e.g., account-origination, customer-profile, document-verification) with its own data store.
- **Built-in resilience** – Use Resilience4j for circuit breakers, retries with exponential backoff, timeouts, bulkheads.
- **First-class observability** – Spring Boot Actuator + Micrometer for metrics, correlation IDs across services, structured logging.
- **Secure by default** – OAuth2/JWT auth, mTLS for service-to-service, secrets in vault, input validation at every layer.

---

## Q2: How do you ensure microservices are loosely coupled and independently deployable?

**Memory Trick:** Own Data → Async When Possible → Versioned APIs → CI/CD per Service

- **Own your data** – Each service owns its database. No cross-service DB joins. If you need someone's data, call their API or consume their event.
- **Prefer async communication** – Use Kafka for non-critical-path workflows. Synchronous calls increase coupling and cascade failures.
- **Versioned APIs** – Backward-compatible changes by default. Breaking changes require new versions with deprecation period.
- **Independent CI/CD pipelines** – Each service has its own pipeline so teams deploy independently.
- **Contract testing** – Pact or Spring Cloud Contract so consumers and providers can evolve safely.

---

## Q3: How do you handle inter-service communication patterns?

**Memory Trick:** Sync for Read → Async for Write → Saga for Distributed

- **Sync (REST) for queries** – When a service needs immediate data (e.g., fetch customer profile during origination).
- **Async (Kafka events) for state changes** – When an action triggers downstream work (account created → trigger KYC, document verification, notification).
- **Saga pattern for distributed transactions** – Long-running workflows like account origination span multiple services. Use choreography (event-driven) or orchestration (a coordinator service) depending on complexity.
- **API Gateway for external calls** – Auth, rate limiting, routing handled centrally.
- **Service mesh (Istio) where applicable** – mTLS, retries, observability without code changes.

---

## Q4: How do you ensure API performance and tune Spring Boot services?

**Memory Trick:** Measure → Optimize Hot Path → Cache → Tune JVM → Async

- **Measure first** – APM (New Relic/Dynatrace), p50/p95/p99 latency, throughput. Don't optimize without data.
- **Optimize the hot path** – Database queries (N+1 is the most common killer), reduce serialization overhead, lazy loading where appropriate.
- **Caching** – Redis for shared cache, Caffeine for in-memory hot data. Always with TTL and eviction policy.
- **Tune JVM** – Right heap size, GC tuning (G1GC default is fine for most), thread pool sizing for Tomcat/WebFlux.
- **Async where it fits** – `@Async`, `CompletableFuture`, or reactive (WebFlux) for I/O-heavy paths.

---

## Q5: How do you handle security in REST APIs?

**Memory Trick:** Auth → AuthZ → Validate → Encrypt → Audit

- **Authentication** – OAuth2/OIDC with JWT tokens, validated at API gateway and re-validated at service for defense-in-depth.
- **Authorization** – Role-based and attribute-based access control. Spring Security with method-level annotations.
- **Input validation** – Bean Validation (`@Valid`, `@NotNull`, custom validators). Reject bad input at the edge.
- **Encryption** – TLS in transit, encryption at rest for PII. Secrets in vault, never in config files or environment variables.
- **Audit logging** – Every privileged action logged with who/what/when/why for compliance and forensics.

---

## Q6: How do you handle errors and exceptions consistently in Spring Boot?

**Memory Trick:** Centralize → Standardize → Surface Cause → No Leaks

- **Centralize handling** – `@ControllerAdvice` with `@ExceptionHandler` for global error handling.
- **Standard error response** – RFC 7807 Problem Details (type, title, status, detail, instance) so clients can parse uniformly.
- **Surface root cause to logs, not response** – Stack traces in logs with correlation ID; clients see safe, generic messages.
- **No information leakage** – Never return DB schema, stack traces, or internal paths in error responses.
- **Map domain errors properly** – `ValidationException → 400`, `AuthorizationException → 403`, `NotFound → 404`. Don't return 500 for everything.

---

## Q7: How do you structure a Spring Boot service for testability?

**Memory Trick:** Layers → Pure Logic → Mock Boundaries → Cover Levels

- **Clear layers** – Controller → Service → Repository, each with single responsibility.
- **Keep business logic pure** – Domain logic doesn't depend on Spring or HTTP. Easier to test and reuse.
- **Mock external boundaries** – Mock DB, Kafka, external APIs in unit tests. Don't mock your own code.
- **Test at multiple levels** – Unit (JUnit5 + Mockito), integration (`@SpringBootTest` + Testcontainers), contract (Pact), end-to-end (selectively).
- **Coverage with intent** – Aim for 80%+ on business logic, less on boilerplate. Coverage is a signal, not a goal.

---

## Q8: How do you handle database access patterns in microservices?

**Memory Trick:** One Service One DB → Right Tool → Connection Pool → Migrations

- **One service, one database** – Each microservice owns its data. Shared DBs create the worst kind of coupling.
- **Right tool for the job** – Oracle/PostgreSQL for transactional and relational data, MongoDB/Cassandra for high-volume or flexible-schema data.
- **Connection pooling** – HikariCP (Spring Boot default) tuned to service load. Connection leaks cause cascading outages.
- **Schema migrations** – Flyway or Liquibase versioned, automated in CI/CD. Never hand-edit production schemas.
- **Read/write separation when scale demands** – Read replicas, CQRS where read and write loads diverge.

---

## Q9: How do you guide your team in writing high-quality Java/Spring Boot code?

**Memory Trick:** Standards → Reviews → Static Analysis → Refactor

- **Coding standards** – Java style guide, naming conventions, package structure documented and consistent across services.
- **Code reviews are mandatory** – Two-level approval (peer + senior/manager for critical paths). Reviews focus on design, not just style.
- **Static analysis in CI** – SonarQube for code smells, complexity, duplication, coverage. Fail builds on critical issues.
- **Continuous refactoring** – Boy Scout rule: leave code better than you found it. Allocate explicit refactor capacity.
- **Pair on hard problems** – Pairing for complex design or new patterns spreads knowledge faster than docs.

---

## Q10: How do you handle versioning and backward compatibility for APIs?

**Memory Trick:** Backward Compatible → URI Version → Deprecate → Sunset

- **Default to backward compatible** – Add fields, don't remove. Make new fields optional. Old clients should keep working.
- **URI versioning for breaking changes** – `/v1/accounts` → `/v2/accounts`. Clear, cache-friendly, easy to route.
- **Document deprecation early** – Deprecation headers, changelog, direct communication to consumer teams.
- **Sunset with notice** – Typical 6-month deprecation period. Track v1 usage to know when it's safe to remove.
- **Avoid breaking changes when possible** – Most "needs a v2" can be done as additive changes with feature flags.

> Strong closing line: *"My job is to make sure our APIs and services are easy to consume, easy to operate, and hard to break."*

---
