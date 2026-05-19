### 6C SECURITY EVENT LOG STANDARD
### 6C Security Event Log 6.2
**Description**
Security event logs should be active at all times and protected from unauthorized access, modification, and accidental or deliberate destruction on all company information resources.

For environments which are in scope for Payment Care Industry (PCI), pleasse refer to 13D PCI Security for additional requirements.

### 6C.1 Log Source Requirements
### Area of Statement
	The following are requirements for different types of security log sources.
	
	### 6C.1.01 Security Device Log Source Requirements
	### Statement
	Audit events are occurences within an information technology system that are significant and relevant to the security of that information technology system and the environments in which it operates. System owners shall determine their technology system's capability to record the following events:
	* Account or system authentication;
	* Account and role management;
	* Resource or function authorization and/or access;
	* Security or access policy changes;
	* Administrator and/or privileged activity;
	* Resource removal and/or deletion; and
	* System security settings or security control changes.
	
	Event Logging must be configured for:
	* Monitoring in security incident event management (e.g., SIEM) platforms approved by Enterprise Information Security (EIS);
	* Retention to support security incident investigations, and security breaches, and Company retention requirements; and
	* Security to prevent tampering prevention.
	
	EIS shall enable functions to monitor, retain, and secure event logging integrated with EIS-managed data systems. Non-EIS event management platform owners must either:
	a) monitor, retain, and secure audit events; or
	b) integrate those systems with EIS-managed data systems.
	
	### 6C.1.02 Audit Event Content
	### Statement
	Audit records must conatin information that establishes:
	* When the event occured;
	* Timestamps must be in RC-3399 including Coordinated Univeral Time (UTC), or UTC with a time zone offset, or formatted in epoch time);
	* What type of event occured, including event detail and event category, where possible;
	* Where the event occured(e.g., hosting environment, network segment, cloud service provider account and/or namespace);
	* Source(s) subject of the event (e.g., host, internet protocol (IP) address);
	* Source system(s) generating the event (e.g., host, internet protocol (IP) address);
	* Outcome(s) of the event (e.g., attempt, success, allow, deny, failure); and
	* Identity associated with the event (e.g., person(s), system(s), account(s)).
	
	### 6C.1.03 Security Log sources
	### Statement
	Security log sources must be enabled as defined by Enterprise Informaiton Security (EIS) Cyber Defense Operations for use in Operations Monitoring defined in 9B:Cyber Defense Operations. Security log events can include logs from the following sources (this is not an all-inclusive list), for example:* Internet Border Firewall servers;
	* Web application Firewall (WAF) servers;
	* Cloud Service Provider Security Products;
	* Endpoint Antivirus Agents;
	* Endpoint Detection and Response Agents;
	* Domain Name servers (DNS);
	* Email Server Simple Mail Transfer Protocol (SMTP) servers;
	* Outbound web gateways and proxy servers;
	* Dynamci Host Control Protocol (DHCP) servers;
	* Active Directory (AD) Domain Controllers;
	* Virtual Private Network (VPN) servers;
	* Vulnerability Scanners;
	* Source code Scanners;
	* Configuration Management Platforms;
	* Specialized IDS/IPS Platforms
	* IaaS Platforms hosting applications subject to 6C.1.04; and
	* SaaS providers hosting applications subject to 6C.1.05;	
	
	Other security log sources may be required to meet customer requirements or compliance controls such as Payment Card Industry (PCI) OR Federal Information Security Management (FISMA) regulations. See Security Log Event Guidelines for examples of security logging Configurations.
	
	### 6C.1.04 IaaS Platform Audit logs
	### Statement
	Cloud service providers must enable technology to record platform user activities, exceptions, and information security events. Audit logs of these events shall be produced and kept for an agreed period to assist in investigations and monitoring.
	
	Platforms are defined as software that manages computer hardware and software resources and provide common services for computer programs (e.g., Linux, Unix, Windows); appliances (e.g., routers, switches, server applinace, storage appliance, network appliance, and client appliance)l or containers(e.g., Docker containers).
	
	### 6C.1.05 Cloud Service Provider Log Management
	### Statement
	In IaaS (Infrastructure as a Service), PaaS (Platform as a Service), and SaaS(Software as a Service) use cases, cloud service provider logs must be configured for monitoring, archival, collection, and streaming to event management (e.g., SIEM) platforms apprvoed by Enterprise Information Security (EIS). Logs must be maintained to support security incident investigations and security breaches. Logs of cloud service providers shall be produces and kept in compliance with [link](6A.3.04 LOG Retention Schedules) to assit in investigations and monitoring.
	
	### 6C.2 Application Security Event logs
	### Area Statement
	The following are requirements for application security event logs. Additional guidence for meeting the requirements in this area can be found in the [link] (Enterprise Informaiton Security (EIS) Application Logging and Monitoring Guidelines and Procedures ) document.
	
	### 6C.2.01 Requirements for Application Security logs
	### Statement
	Applications must record required security events into log(s). Applications that are one or more of the following are required to submit log entries for required security events to an approved [link] (security log repository):
		* are public-facing; or
		* store, process or transmit [link] (Protected or Confidential Informaiton); or
		* are designated as Critical Business Applications; or
		* have contractual requirements which require logging and monitoring.
		
	Applications that do not meet the above criteria should include approriate security events as part of their reqular operational logging practices. Additional guidence for meeting this requirement can be found in the [link] (Enterprise Informaiton Security (EIS) Application Logging and Monitoring Guidelines and Procedures ).
	
	### 6C.2.02 Application Log Required Security Events
	### Statement	
	Application security logs must include events, applicable to the application, that are relevant to security and audit needs. The functionality of the individual application will determine what events must be logged; other events of interest for security and audit purposes may be included in logging.
	
	Log entries must contain information recording what ever occured, when it occured, wher it occured, the outcome, the source system of the log, and the identity of any entities or individuals associated with the event. To enagage Enterprise Informaiton Security (EIS), submit a request ticket to the [Link] (Informaiton Security Office).
	
	See the [link] (Enterprise Informaiton Security (EIS) Application Logging and Monitoring Guidelines and Procedures ) document for examples of required security event attributes.
	
	
	