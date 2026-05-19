# 11B Application Security Validation

### 11B Application Security Validation 4.1
Description
This Security Standard contains the requirements for vulnerability management on applications 
hosted in both traditional/on-premises and cloud environments. Applications must be assessed
for vulnerabilities to protect against attacks and ensure confidentiality, integrity and 
availability of UnitedHealth Group and its Business Organizations (hearafter referred to as 
"Company" or "the Company") developed applications prior to deployment and throughout the 
application lifecycle until fully decommissioned. Web or mobile applications must be assessed
for security vulnerabilities and must be remediated prior to production deployment.

For environments that are in scope for the Payment Card Industry (PCI), please refer 13D PCI
Security for additional requirements.

## 11B.1 General Application Security Testing requirements
Area Statement
The following are general requirements for application vulnerability security testing.

#### 11B.1.01 Approved Application Security Testing Solutions
Statement

Application security vulnerability testing must be completed using either Preferred or 
Acceptable tools as defined in Technology Landscape/PADU model.

Testing tools must be leverage up-tod-date versions, rules, and/or knowledge bases.

Application vulnerability testing should be performed in accordacne with the 
Open Web Application Security Project(OWASP TM) industry-best-practice guidelines.

Application Security Testing tools fall into the following categories:

#table

| Acronym | Full Name | Description|
|---------|-----------|------------|
| <ul><li>DAST</li><li>Dynamic Application Security Testing</li><li>Detect security vulnerability in an application in its running state </li></ul> |
| <ul><li>SAST</li><li>Static Application Security Testing</li><li>Analyze application from the "inside out" in a non-running state </li></ul> |
| <ul><li>PEN</li><li>Enterprise Penetration Testing</li><li> Testing a computer, network, or web app that an attacker can exploit </li></ul> |
| <ul><li>SCA</li><li>Software Composition Analysis</li><li> Automating visibility into the 
use of open source software for risk management, security and license compliance </li></ul> |

## 11B.2 Application Security Testing Scope
Area Statement
Application developers should make their applications as secure as possible by using application security testing tools and processes to check for vulnerabilities at a level of testing commensurate to the application's risk.

Applications that do not have a web, mobile, or application programmable interface such as mainframe or desktop applications are out of scope for application security testing. Followin are the applications covered by this Secruity Standard.

## 11B.2.01 Web Portals
	Statement	
	Web portals developed and/or hosted by the Company or its subsidiaries must be assessed for application vulnerabilities as outlined in this Secruity Standard.
## 11B.2.01 Web Services including APIs (Application Programming Interfaces)
	Statement	
	Web services including APIs developed and/or hosted by the Company or its subsidiaries must be assessed for application vulnerabilities as outlined in this Secruity Standard.
## 11B.2.01 Mobile platforms
	Statement	
	Mobile applications (available in public app stores) developed and/or hosted by the Company or its subsidiaries, and mobile appliation interfaces to Company back-end systems, must be assessed for application vulnerabilities as outlined in this Secruity Standard.
	
## 11B.3 Application risk
Area Statement
The level of risk to an appliation is based ob factors such as whether the appliation is accessible externally, whether the appliation is considered  business critical, and whether the application contains Protected or Confidential information. The level of application risk determines the amount of security testing that is appropriate for an appliation.

## 11B.3.01 Application risk categories
Statement
Enterprise Information Security (EIS) has established risk categories for web applications that describe the level of risk to the Company. The Categorization determines the required security vulnerability testing methodology for each appliation.

Under this Standard the Application owner is responsible to review and confirm the elements in the AIDE Database regarding their application are correct.

If an appliation does not fit into any of the following categories, the EIS Organization will be contacted to review the application, correctly categorize, and ensure appropriate testing is determined.

| Application risk categorization |
| Application Risk | Application Orientation | Definition Criteria|
|------------------|-------------------------|--------------------|
| <ul><li>Category 1</li><li>External Facing </li><li>Critical Business Application (CBA) </li></ul> |
| <ul><li>Category 2</li><li>External Facing </li><li> All other externally facing web, API, and mobile apps </li></ul> |
| <ul><li>Category 3</li><li>Internal Facing Only</li><li>Critical Business Application (CBA) </li></ul> |
| <ul><li>Category 4</li><li> Internal Facing Only </li><li>All other internally facing web and API apps </li></ul> |

Appliation Orientation Classfication:
External facing includes mobile applications available in public app stores and any web or API application hosted within a public cloud environment regardless of user access restrictions.
Internal facing are web or API applications that are hosted on-premise or in a private cloud environment with no user access from the public internet.

## 11B.3.02 Application Testing Requirements for Contractual Agreements and Regulatory Programs
Statement
Applications that must meet specific Contractual testing requirements or htat are in scope for regulotory programs such as Payment Card Industry (PCI), Internal Control over Financial Reporting (ICFR), or any other regulatory program  must at a minimum comply with 11B testing requirements and additionally, with the respective Contractual and regulatory requirements. Those overseeing contractual agreements or compliance programs must inform application owners and EIS of any requirements that are in addition to the 11B appliation Security testing requirements on a yearly basis and with enough time to allow proper planning.

## 11B.3.03 Retention of Application Secruity Testing Arifacts
Statement
Testing evidence, vulnerability findings, and remediation statuses must be retained for each code delivery and made available for audit and centralized oversight.

## 11B.4 Appliation Secruity Testing Categories and Requirements
Area Statement
The following are requirements for different types of security testing.

## 11B.4.01 Static Application Secruity Testing (SAST) Requirements
Statement
Source code must be tested for vulnerabilities starting form the application coding phase adn on an ongoing basis thereafter. Prior to code release into a produciton environment, critical and high vulnerabilities found during routine testing and/or testing that is required prior to code release, must be remediated. Failure to remediate code containing medium to high-risk vulnerabilities cannot be promoted to a produciton environment.

| Application Security Testing | Cat 1 | Cat 2| Cat 3 |Cat 4 |
|------------------|-------------------------|--------------------|
| <ul><li>SAST 1</li><li>Required (Per Release) </li><li>Required (Per Release) </li><li>Required (Per Release) </li><li>Required (Per Release) </li></ul> |

Validation is required to confirm that vulnerabilities have been remediated and no new critical or hig vulnerabilities have been introduced into the code base prior to release to produciton.

## 11B.4.02 Software Composition Analysis (SCA) Requirements
Statement
Application dependencies must be assessed for vulnerabilities with third-party libraries, frameworks or components from the application coding phase and on an ongoing basis thereafter. If your application depends on a package with security vulnerability, you must upgrade to an available secure verions of the package. Detected critical and high vulnerabilities must be remediated prior to being released into produciton.

| Application Security Testing | Cat 1 | Cat 2| Cat 3 | Cat 4 |
|------------------|-------------------------|--------------------|
| <ul><li>SCA 1</li><li>Required (Monthly) </li><li>Required (Monthly) </li><li>Required (Monthly) </li>li>Required (Monthly) </li></ul> |


## 11B.4.03 Dynamic Application Security Testing (DAST) Requirements
Statement
Web appliations including APIs developed and/or hosted by the Company or its subsidiaries must be tested for vulnerabilities starting from the appliation coding phase and on on ongoing basis thereafter. Detected critical and high vulnerabilities must be remediated prior to being released into produciton.

| Application Security Testing | Cat 1 | Cat 2| Cat 3 | Cat 4 |
|------------------|-------------------------|--------------------|
| <ul><li>DAST 1</li><li>Required (Monthly) </li><li>Required (Monthly) </li><li>Required (Monthly) </li>li>Recommended </li></ul> |

Accurate credentials must be  provided to allow the DAST tool to assess the entire Appliation Security posture. Application Owners should facilitate and support the setup and maintenance of the DAST tool. credentials must be securely managed, stored, and shared with Cyber Defense Organization or within the DAST tools.


## 11B.4.04 Penetration Testing (PEN) Requirements
Statement
Application penetration tests are required in accordance with the below table based on the application's risk category.

When annual tsting is required, application penetration tests are performed at minimum of once per year. Testing is performed by the Enterprise Information Secruity (EIS) Cyber Defence Organization (CDO) Ethical Hacking Team or a CDO-approved third party supplier.

Application penetration Testing must be  performed with credentials that imitatethe access level fo an authorized application user.

Any appliation may be selected at any time by EIS for a Penetration test. Application owners are required to provide the necessary resources to conduct the penetration test.


| Application Security Testing | Cat 1 | Cat 2| Cat 3 | Cat 4 |
|------------------|-------------------------|--------------------|
| <ul><li>PenTest 1</li><li>Required (Annually) </li><li>Required (Annually) </li><li>Required (Annually) </li>li>Not Required </li></ul> |


## 11B.5 Appliation Vulnerability Remediation
Area Statement
This are outlines the timelines by which discovered appliation security vulnerabilities must be remediated, the process for remidiation validation and closure, and responsibility for remediation.

## 11B.5.01 Appliation Vulnerability Remediation timelines
Statement
Vulnerabilities are required to be remediated within the timeframe associated with the severity level assigned, starting with the date the vulnerability was discovered. Detected critical and high vulnerabilities must be be remediated prior to being released into produciton. Remediation timelines by severity level are as follows, in calendar days:

| Critical | High  | Medium| Low | Note / Informational |
|------------------|-------------------------|--------------------|
| <ul><li>5 days 1</li><li>90 days </li><li>120 days </li><li>180 days</li>li>N/A</li></ul> |

## 11B.5.02 Appliation Vulnerability Remediation Validation and closure
Statement
A vulnerability will be marked as remediated/closed only after subsequent testing has confirmed the application vulnerability is no longer present.
Appliation source code must adhere to Company quality standards in order to deliver clean and secure that is free from false positive testing results.
Vulnerabilities cannot be considered closed when they reside in unused or non-produciton code.

