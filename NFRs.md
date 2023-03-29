# NFRs
Non-functional requirements refer to the criteria that describe the system's operation and behavior, rather than its specific features or functionalities. These requirements typically include things like performance, reliability, usability, security, maintainability, and scalability. Non-functional requirements are just as important as functional requirements because they define how well the system will work and how easy it will be to maintain over time.

Examples of non-functional requirements include:

#### 1. Performance: The system must be able to handle a certain number of requests per second or provide results within a specific time frame.
   List out some Non-Functional Requirements that you have come accross. Story.
   Performance - Software System - CPU, Memory, Network, IOPS
   100 Concurrent Requests -  REST API Method - Single Thread - 1 Sec to execute
   1 -> Locks
   2-100 are waiting
   Response Time
   100 Request -> REST API - 100 Threads
   1 Sec
   Better Response Time - Cost of Memory - 1 Thread - 1MB of Memory
   Throughput
   In-Memory Processing (No Disk IO)
   Scalability - Horizontal (Scale Out) - Vertical (Scale up)
   High Availability - DR, Replication (Replicaset),
   UX -
   Data Latency/Query Latency
   Graceful Degradation-
   Maintainnability, Servicea bility
   Diagnostics, Fault Tolerance, Resiliance _ Reliability Engg
   Safety
   Security - Authentication, Authorization, Injection Attaches, XSRF, CORS, EncDecrypt, Data at REST, DDOS

#### 2. Reliability: The system must be available for a certain percentage of the time and should be able to recover from any failures quickly.

#### 3. Usability: The system should be easy to use and navigate for the intended users.

#### 4. Security: The system must be secure and protect sensitive information from unauthorized access or data breaches.

#### 5. Maintainability: The system must be easy to maintain and update as required.

#### 6. Scalability: The system must be able to handle increases in usage without a significant decrease in performance or reliability.

#### 7. Compatibility: The system should be compatible with different devices, operating systems, and browsers.

#### 8. Accessibility: The system must be accessible to users with disabilities and meet the requirements of accessibility laws and regulations.

#### 9. Interoperability: The system should be able to integrate with other systems and exchange data seamlessly.

#### 10. Regulatory Compliance: The system should comply with applicable laws, regulations, and industry standards.

