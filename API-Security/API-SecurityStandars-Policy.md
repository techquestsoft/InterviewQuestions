# 11 D Application Programming Interface(API) Security
Description
All APIs must adhere to the security guidelines outlined in this policy. This Policy is established
to ensure the security and integrity of our APIs.

# 11 D.1 Application Programming Interface(API) Security Categories
Area Statement
This security standard applies to APIs that communicate over a network including, but not limited to 
Web APIs(http(s)), SOAP, REST, graphql, IOT APIs(COAP, MQTT).

APIs managed, owned, or developed by the Company or its subsidiaries must be assessed for vulnerabilities
 as outlined in this Security Standard. This includes APIs within third-party owned applications both 
 commercial and opensource regardless of whether it is QuestSoft or third-party hosted.
 
All APIs must follow the same risk categorization that applies to applications per (11B.3 Application 
Risk) regardless of protocol, such as SOAP and REST. For the purpose of this policy, and API is defined 
as software interface routines, protocols, and tools that allow applications to communicate with each 
other.

# 11 D.1.01 Application Programming Interface(API) Solutions
Statement
The risk categories 11B.3.01 Application Risk Categories apply to API solutions. This categorization 
determines the required security methodology that must be applied to each API.

# 11 D.2 Application Programming Interface(API) Authentication and Authorization
Area Statement
All APIs must be equipped with robust authentication methods to confirm the identity of users accessing 
the application, API, and data. Additionally, sufficient authorization checks following the principle of 
least privileged (minimum necessary) must be in place to ensure that only approved users can the 
resources they have permissions for.

# 11 D.2.01 Application Programming Interface(API) requirements
Statement
API authentication and authorization requirements are based on OWASP Top 10 API 2023 security risks 
tailored to the Enterprise.

1. Data Protection
    a. Protected information must be encrypted during transit and at rest. See 13A Data Classification 
    and protection for data classification definitions.
    b. All API data transfers must ensure only approved and minimum necessary data is included.
2. Rate limiting
    a. All APIs must implement rate limits
3. Injection prevention
    a. All inputs must be validated at schema and at the DB layer to prevent injection attacks
4. Error Handling
    a. Follow http error core standards
    b. Must never expose implementation details (error stacktrace, DB table name, fine name, 
    infrastructure)
5. Security logging
    a. All APIs must log security events company approved application logging platform per 6C.2 
    Application Security Event logs
6. CORS
    a. Cross-origin resource sharing (CORS) must be configured and tested to prevent access from 
    different domains
7. Business logic flaws:
    a. APIs must implement appropriate business logic validation to prevent unauthorized data access.
8. Broken function level authorization
    a. APIs must enforce proper authorization at resource/function level and each consumer can access only resources they are authorized for
9. Transport Layer Security (TLS)
	A. All communications between API Consumer and provider through gateway must be encrypted using strong TLS protocol and cipher suites.
10. Third party dependencies
	a. APIs must carefully vet and monitor the thrid-party dependency for security vulnerability
11. Multifactor Authentication (MFA)
	a. All APIs that involve admin/privileged access must implement multi factor authentication 8C Identification and Authentication
12. vulnerability/Patch Management
	a. APIs must address every security finding per 11B.5.01 vulnerability remediation timelines for patch management 5H.1.03 Remediation adn Escalation security standards
13. gateway
	a. All APIs must go through approved Enterprise gateway (11D.3)
 
 
  