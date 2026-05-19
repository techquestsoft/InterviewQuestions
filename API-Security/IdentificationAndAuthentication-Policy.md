### 8C IDENTIFICATION AND AUTHENTICATION STANDARD
### 8C Identification adn Authentication 21.1
**Description**
TechQuestSoft AND its Business Organizations (here referred to as "Company" or "the Company") requires that access to all Protected and Confidential information be implemented as defined by the requirements within this security to ensure appropriate identification and authentication.

Workforce or Workforce Members include Company employees and other non-employee workers such as third-party contractors, service professionals, and temporary staff. TechQuestSoft Group employees and non-employee workers will be collectively referred to as "Workforce" or "Workforce Member" although the use of term does not establish a labor relationship with the Company.

For environments which are in scope for Payment Card Industry (PCI), please refer 13D PCI Security for additional requirements.

### 8C.1 Identification and Authentication
### Area of Statement
	The following are identification and authentication requirements.
	
	### 8C.1.01 Establishing an Identity
	### Statement
	Any individuals or systems that access any Company network, or access Protected or Confidential Information owned or managed by the Company must have their identity validated and access authorized by the appropriate manager or delegate prior to accessing requested resources. The acceptability, validation, and verification of identity evidence must meet the requirements of the type of resource access required.
	
	### 8C.1.02 User Identification and Authentication
	### Statement
	Company information technology systems must uniquely identify and authenticate each individual and system seeking access using an approved Identity Provider (IDP). Approved IDPs are defined in the [link](Technology Landscape or PADU).
	
	Access to Company digital properties by members and consumers will be provisioned through the Company digital identity solution(s) after their identity is validated as required in the Identity Data Elements Guide. Company Information technology system must utilize the Company identity mastery system for all interactions with consumers and members.
	
	### 8C.1.03 Mulit-Factor Authentication Definition and Approval
	### Statement
	Mulit-Factor authentication (MFA) is an element of layered security controls to reduce risk associated with high-risk online activities. MFA must have at least tow (2) of the three (3) following factors.
	
		* Something the user knows, e.g., a passcode or PIN;
		* Something the user has, e.g., a smart card, electronic token, or registered devide; and
		* Something user is e.g., a biometric characteristic, such as facial identity, fingerprint or voice imprint, an indicator or identify aligned to modern identity resolution strategies.
		
	All Mulit-Factor Authentication (MFA) patterns must be approved by Enterprise Information Security (EIS) prior to implementation.
	
	
	### 8C.1.04 Risk-Based Authentication (RBA) Definition
	### Statement
	Risk-Based Authentication (RBA) solutions evaluate risk that the person attempting to authenticate is who they claim to be. Based on the risk evaluation, RBA determines whether to accept the credentials presented or challenges/steps up the authentication requirements. Each RBA solution must be approved by Enterprise Information Security (EIS) prior to implementation.
	
	See the [link](Technology Landscape or PADU) for approved RBA patterns.
	
	### 8C.1.05 Mulit-Factor With Risk-Based Authentication Required
	### Statement
	Company information technology systems that grant access to digital information to individuals, must utilize a Company-approved MFA pattern. Company information technology systems may rely on previously authenticated users if access is only granted after the user has authenticated through a Company MFA pattern. Company systems should align to Enterprise preferred authentication tools to leverage adavanced features including MFA, RBA, Bot Mitigation, and other centrally managed business and security tools. The business should demonstrate that they have made an effort to align to enterprise standards in a timely manner.
	
	The company approved patterns are defined in the [link]( Technology Landscape) site.
	
	Mulit-Factor Authentication is required at TechQuestSoft Group on all user authenticated systems. Refer to the grid below for allowed configurations.
	
	| Best						    | Better			| Good			| Acceptable			| Unacceptable			|
	|-------------------------------|-------------------|---------------|-----------------------|-----------------------|
	| Enterprise IDP (Identity Provider) with MFA with Biometric authentication + RBA, Bot mitigation, Identity Proofing and Enterprise Identity Mastering|MFA with Biometric authentication + RBA + Bot Mitigation| MFA via Bound Device + RBA + Bot mitigation| MFA via SMS | MFA via email |
	
	|Enterprise Preferred IDP allows enterprise to react quickly and comprehensively to new risks, Biometric is the strongest authentication. Users are still required to complete initial login with a second factor. Bot mitigation significantly reduces risk of cred stuffing attacks.| Biometric is the strongest authentication. Users are still required to complete initial login with a second factor this prevents simpler SMS compromise attacks, Bot Mitigation significantly reduces risk of cred stuffing attacks.| Tightly connect device fingerprint to authentication and prevents simpler SMS compromsie attacks. Bot Mitigation significantly reduces risk of cred stuffing attacks.| SMS or Automated callback with security code. SMS is an older standard that lacks modern encryption standards and can be redirected. Broadly available to consumers and allows easy entry. | Easily compromised. Email accounts can be shared or taken over without awareness of the user. Broadly discouraged by the security community.|
	
	|Requires smart phone with biometric capability| Requires smart phone with biometric capability| Requires smart phone | Requires phone | N/A |
	
	| MFA Mandatory | MFA Mandatory | MFA Mandatory |  MFA Mandatory |  MFA Mandatory | 

	MFA must have at least two (2) of the three (3) following factorsL
	
		* Something the user knows, e.g., a passcode or PIN;
		* Something the user has, e.g., a smart card, electronic token, or registered devide; and
		* Something user is e.g., a biometric characteristic, such as facial identity, fingerprint or voice imprint, an indicator or identify aligned to modern identity resolution strategies.
		
	MFA token processes must disable users with 90 days of inactivity, and token must be unassigned from users upon 90 days of inactivity. Certificates issued in support of MFA tokens must expire within 90 days of the original issuance.
	
	### 8C.1.06 Reply Resistant Mechanisms
	### Statement
		Some content goes here
	
	### 8C.1.07 User Identification (ID) Assignment
	### Statement
		Some content goes here
	
	### 8C.1.08 Use of Commonly Known Identifiers (IDs) for Company User IDs is Prohibited
	### Statement
		Some content goes here
	
		
	### 8C.1.09 Dynamic Identifier Management
	### Statement
		Some content goes here
	
	### 8C.1.10 Secrets Management
	### Statement
		Some content goes here
	

	### 8C.1.11 Dynamic Credential Binding
	### Statement
		Some content goes here
	
	
	### 8C.1.12 Identification and Authentication Audit Logging and Monitoring
	### Statement
		Some content goes here
	
### 8C.2 Service and Previleged Accounts
### Area Statement
	Some content goes here
	
	### 8C.2.01 Controls for Non-User Identities (IDs)
	### Statement
		Some content goes here

	### 8C.2.02 Controls for Service Accounts
	### Statement
		Some content goes here		
		
	### 8C.2.03 Designated Privileged Security Groups
	### Statement
		Some content goes here	
		
	### 8C.2.04 Default Accounts
	### Statement
		Some content goes here	
		
### 8C.3 Common Identity Web Single Sign On (SSO)
### Area Statement
	Some content goes here
	
	### 8C.3.01 Use of Approved Single Sign-On solution
	### Statement
		Some content goes here

	### 8C.3.02 Approved Federation Protocol and Implementation
	### Statement
		Some content goes here		
		
### 8C.4 Memorized Secrets
### Area Statement
	Memorized Secrets includes passwords, passphrases, and personal identification numbers (PINs).
	
	**Password :** A memorized secret consisting of a string of characters (letters, numbers, and other symbols) that are used to authenticate an identity.
	
	**Passphrase :** A memorized secret consisting of a sequence of words or other text that are used to authenticate an identity. A passphrase is similar to a password in usage but is generally longer for added security and is considered easier to remember.
	
	**Personal Identification Number (PIN) :** A memorized secret consisting of a string of characters (letters, numbers, and other symbols) that are used to authenticate an identity where the PIN is associated with a device, such as Smart card, Hardware-based authenticator, or trusted platform module (TPM).
	
	** One Time Password :** A secret that is automatically generated and is used to authenticate a user for a single login or transaction.
	
	** Temporary Access Passcode (TAP) :** Similar to an OTP, in that it is automatically generated to enable temporary user authentication but can offer additional capability such as re-use of the passcode during a specific time frame.
	
	### 8C.4.01 Memorized Secrets Generalized Requirements
	### Statement
		Some content goes here

		### 8C.4.01.01 Protection of Authenticators Statement
		### Statement
			Some content goes here		

		### 8C.4.01.02 Transmitting Secrets
		### Statement
			Some content goes here			
			
		### 8C.4.01.03 Unencrypted Static Secrets Prohibited
		### Statement
			Some content goes here		

		### 8C.4.01.04 Secret Reuse on Multiple Information System Accounts
		### Statement
			Some content goes here			
			
		### 8C.4.01.05 Secrets Masking or Suppression
		### Statement
			Some content goes here			
			
		### 8C.4.01.06 Re-Authenticating Users for Secret Resets
		### Statement
			Some content goes here			
			
	### 8C.4.02 Passwords adn Passphrases
	### Section Introduction
	Some content goes here	
	
		### 8C.4.02.01 Default and Initial Passwords and Passphrases
		### Statement
		Some content goes here	

		### 8C.4.02.02 Boot Settings
		### Statement
		Some content goes here		
		
		### 8C.4.02.03 Compromised Password and Passphrase (Memorized Secret) Verification
		### Statement
		Some content goes here
		
		### 8C.4.02.04 Consumer Applications
		### Statement
		Some content goes here		
		
		### 8C.4.02.05 Pasting Secrets
		### Statement
		Some content goes here
		
		### 8C.4.02.06 Encryption of Secrets in-Transit and at Rest
		### Statement
		Some content goes here		
		
	
	### 8C.4.03 Memorized Secrets Length and Expiration
	### Section Introduction
	Some content goes here		
	
		### 8C.4.03.01 Non-Previleged User Account Requirements
		### Statement
		Some content goes here		
		
		### 8C.4.03.02 Non-User Account Requirements
		### Statement
		Some content goes here		
		
		### 8C.4.03.03 Non-User Account with SPN Requirements
		### Statement
		Some content goes here
		
		### 8C.4.03.04 Previleged Account Requirements
		### Statement
		Some content goes here	

		### 8C.4.03.05 Active Directory Control Plane Account Requirements
		### Statement
		Some content goes here		
		
		### 8C.4.03.06 Non-User Secure Kiosk Requirements
		### Statement
		Some content goes here	
		
	### 8C.4.04 Personal Identification Numbers (PINs)
	### Section Introduction
	Some content goes here	
	
		### 8C.4.04.01 Default and Initial PINs
		### Statement
		Some content goes here	

		### 8C.4.04.02 PIN Composition
		### Statement
		Some content goes here			
		
		### 8C.4.04.03 PIN Length
		### Statement
		Some content goes here	
		
		### 8C.4.04.04 PIN pasting
		### Statement
		Some content goes here	
		
		### 8C.4.04.05 PIN Encryption
		### Statement
		Some content goes here

		### 8C.4.04.06 Change/Reset Intervals
		### Statement
		Some content goes here			
		
### 8C.5 End-Point Mulit-Factor Authentication
### Area Statement
Some content goes here	

	### 8C.5.01 End-Point Mulit-Factor Authentication General Requirements
	### Statement
	Some content goes here	
		
	### 8C.5.02 Smart Card or USB Token Authentication
	### Statement
	Some content goes here			
	
### 8C.6 Biometric Based Authentication
### Area Statement
Some content goes here	

	### 8C.6.01 Biometric Based Authentication 
	### Statement
	Some content goes here	
	
	### 8C.6.02 Use of Biometric Based Authentication on Endpoints
	### Statement
	Some content goes here		
	
### 8C.7 Public and Private Key-Based Authentication
### Area Statement
Some content goes here	

	### 8C.7.01 Public-Key-Based Authentication 
	### Statement
	Some content goes here	

	### 8C.7.01 Private-Key-Based Authentication 
	### Statement
	Some content goes here		