# Cybersecurity Concepts and Tools

## Security Baselines
Security baselines are minimum security standards for computing systems, including configurations, settings, and controls. These standards must be consistently applied across all devices to maintain a secure environment.

### Implementation of Baselines
1. **Establish the Baseline**: Define the minimum acceptable security configurations.
2. **Deploy the Baseline**: Apply the baseline settings uniformly across all systems.
3. **Monitor and Manage Baselines**: Continuously check for compliance and update baselines as needed.

---

## Patch Management
Patch management involves applying patches and updates to operating systems and applications to fix security vulnerabilities and bugs, reducing the attack surface and preventing exploitation.

---

## Configuration Management Tools
Configuration management tools automate and enforce standardized configurations and patch deployments across multiple systems, ensuring consistency, security, and compliance.

---

## System Hardening
System hardening is the process of analyzing and adjusting default settings of an operating system by removing unneeded services and applications, and securing default accounts to reduce vulnerabilities.

---

## Anti-Malware Software
Anti-malware software detects and removes malware through two primary techniques:
- **Signature-Based Detection**: Identifies known threats using a database of malware signatures.
- **Behavior-Based Detection**: Monitors system activity for suspicious behavior indicative of new or unknown types of malware.

---

## Endpoint Detection and Response (EDR) / Extended Detection and Response (XDR)
- **EDR**: Tools that monitor and analyze endpoint activity in real-time to detect, investigate, and respond to security threats.
- **XDR**: Extends EDR capabilities across endpoint, network, and server systems, providing a broader view of security threats.

---

## Sandboxing
Sandboxing isolates untrusted or suspicious code in a controlled virtual environment, allowing it to run safely without affecting the system. This helps in observing and analyzing the code’s behavior.

---

## Spam Filtering
Spam filtering uses algorithms and rules to detect and block unwanted or potentially harmful email messages, such as phishing attempts or malware-laden messages.

---

## Malware Log Management
Malware logs should be sent to a **Security Information and Event Management (SIEM)** system for centralized analysis, detection of trends, identification of threats, and triggering alerts.

---

## Application Controls
Application controls include:
- **Allow Lists**: Specify approved applications.
- **Deny Lists**: Block unauthorized applications.
These measures prevent the use of unapproved or potentially harmful software.

### Application Control Logs
Application control logs should also be sent to a **SIEM** system, where they are analyzed for visibility, compliance, and detection of unauthorized software usage.

---

## Host Software Baselining
Host software baselining involves creating a list of authorized software for a system and continuously monitoring for unauthorized changes or new applications. Unauthorized applications are flagged for further investigation.

---

## Firewalls

### Network Firewalls
Network firewalls are hardware devices that act as barriers between trusted internal networks and untrusted external networks, monitoring and controlling incoming and outgoing traffic based on predefined security rules.

### Host Firewalls
Host firewalls are software-based firewalls installed on individual devices that manage inbound and outbound traffic at the host level, providing additional protection tailored to that specific system.

### Configuring Network Access
To grant network access, both **network firewalls** (for external traffic control) and **host firewalls** (for internal traffic control) must be configured to allow specific traffic as needed.

---

## Next-Generation Firewall (NGFW)
A Next-Generation Firewall is an advanced firewall that filters traffic not only based on IP and port but also using deep packet inspection, application-level filtering, and intrusion prevention to provide enhanced protection.

---

## Intrusion Detection and Prevention Systems

### IDS (Intrusion Detection System)
An IDS passively monitors network or system traffic and generates alerts when suspicious or malicious activity is detected, without blocking the activity.

### IPS (Intrusion Prevention System)
An IPS monitors traffic like an IDS but takes proactive action by blocking or mitigating threats in real-time, preventing attacks from succeeding.

---

## File Integrity Monitoring (FIM)
FIM is a security technique that checks important system and application files for unauthorized modifications by comparing current file hash values to known-good baselines.

---

## Data Loss Prevention (DLP)
DLP tools and policies are designed to detect and prevent unauthorized access, transmission, or disclosure of sensitive data such as personal, financial, or proprietary information.

### Host-Based vs. Network-Based DLP
- **Host-Based DLP**: Installed on individual endpoints to monitor how sensitive data is accessed, stored, or transferred locally.
- **Network-Based DLP**: Monitors and inspects data in motion across the network to detect and prevent unauthorized data transfers.

### Pattern Matching in DLP
DLP systems use pattern matching to detect sensitive data using predefined patterns (e.g., credit card numbers or Social Security numbers) to prevent data exfiltration.

---

## Key Management and Encryption

### KMS (Key Management System)
A Key Management System securely handles the creation, distribution, storage, rotation, and revocation of cryptographic keys throughout their lifecycle to protect sensitive information.

### AES Crypt
AES Crypt is a command-line tool for Linux that uses the Advanced Encryption Standard (AES) algorithm to securely encrypt and decrypt files, protecting data at rest.

### Full-Disk Encryption (FDE)
FDE encrypts all data on a drive, including system files, user files, and temporary files, ensuring that data is unreadable without the correct credentials or encryption key.

---

## Hardware Security Modules (HSM) and Trusted Platform Modules (TPM)

### HSM (Hardware Security Module)
An HSM is a physical device designed to securely generate, store, and manage cryptographic keys, and to perform encryption, decryption, and digital signing operations.

### TPM (Trusted Platform Module)
A TPM is a hardware chip embedded in a computer that securely stores cryptographic keys and ensures system integrity through secure boot and encryption.

---

## Self-Encrypting Drives (SED)
SEDs are storage drives with built-in hardware encryption that automatically encrypt and decrypt data without user intervention, enhancing security with minimal performance impact.

---

## Firmware and Secure Boot

### BIOS (Basic Input/Output System)
BIOS is firmware built into a computer’s motherboard that initializes hardware components and loads the bootloader from disk to start the operating system.

### UEFI (Unified Extensible Firmware Interface)
UEFI is a modern replacement for BIOS that supports larger storage devices, faster boot times, and enhanced security features, including Secure Boot.

### Secure Boot
Secure Boot is a UEFI feature that verifies the integrity and authenticity of the bootloader using digital signatures before the OS loads, preventing rootkits and boot-level malware.

# Security and System Integrity

## Measured Boot
- A process in which each component of the boot process (from firmware to OS) is measured (hashed) and verified against trusted values to ensure system integrity.

## Attestation
- A process where a device’s measured boot data is securely stored (typically in the TPM) and can be used to prove to remote systems that the device booted in a secure and unmodified state.

## Hardware Root of Trust
- A foundational security mechanism built into hardware that ensures only trusted and verified firmware and software can run during the system boot process.

## Secure Enclaves
- Isolated hardware-based execution environments within a processor that protect sensitive data and encryption keys, even from higher-privilege software like the OS.

## Processor Security Extensions
- Special hardware features that create secure, isolated memory areas for applications, protecting data and code from being accessed or tampered with by other processes.

## Atomic Execution
- A computing principle ensuring that operations are completed entirely or not at all, preventing inconsistent or partial changes that could lead to data corruption or security flaws.

# Electromagnetic Interference and Threats

## EMI (Electromagnetic Interference)
- Disruptive electromagnetic signals that can interfere with the operation of electronic devices, potentially affecting performance or causing errors.

## EMP (Electromagnetic Pulse)
- A burst of electromagnetic radiation that can damage or disable electronic systems, often caused by natural phenomena or specialized weapons.

# Linux Commands and Permissions

## Linux File Ownership & Permissions
- `chown` - Changes the owner of a file or directory.
- `chgrp` - Changes the group ownership of a file or directory.
- `chmod` - Changes the permissions that control who can read, write, or execute a file or directory.

### Permission Modifiers:
- `u` - User (file owner)
- `g` - Group (users in the same group as the file)
- `o` - Others (everyone else)

# Web Filtering

## Agent-Based Web Filtering
- A method of web content filtering where software is installed directly on user devices to monitor and restrict web access based on policy.

## Centralized Proxy Filtering
- Routes internet traffic through a proxy server that inspects and filters content, enforcing web usage policies from a single centralized location.

# Change and Configuration Management

## Change Management
- A formal process organizations use to ensure changes to systems and infrastructure are introduced in a controlled and coordinated manner to reduce risk and ensure accountability.

## Request for Change (RFC)
- A formal proposal for making a change that includes detailed documentation of the change’s purpose, potential impact, associated risks, responsible personnel, and a rollback plan in case of failure.

## Configuration Management
- Tracks and maintains the consistency of system configurations across hardware and software to ensure integrity, performance, and compliance with policies.

### Baselines in Configuration Management
- A fixed reference point that captures the state of a system or application configuration at a specific time, used for comparison, auditing, and troubleshooting.

# Data Management and Sanitization

## Deleting Files
- The operating system removes the file's reference from the file system, but the actual data remains on the disk until it is overwritten, making it potentially recoverable.

## Data Sanitization
- A process that securely overwrites or destroys data to ensure it cannot be recovered by any means, commonly used before repurposing or disposing of hardware.

# Wireless Networks

## Wi-Fi Networks
- Wireless local area networks (WLANs) that provide high-speed internet access over short ranges, typically used in homes, offices, and public hotspots.

## NFC (Near-Field Communication)
- A short-range wireless technology that uses electromagnetic induction to allow devices within a few centimeters to communicate, often used for contactless payments and access control.

# Mobile Device Management and Security

## Mobile Device Management (MDM)
- Centralized software that enforces security policies, deploys apps, and manages mobile devices used in an organization to ensure compliance and protect corporate data.

## Storage Segmentation
- Separates personal data from corporate data on a mobile device, allowing secure management of work-related content without affecting the user's private information.

## BYOD Technical Challenges
- Security concerns when allowing personal devices, including inconsistent antivirus protection, lack of timely patching, camera usage risks, and difficulty in conducting forensic analysis.

# Virtual Desktop and Wi-Fi Technologies

## Virtual Desktop Infrastructure (VDI)
- Technology that hosts user desktops in virtual machines on a centralized server, allowing secure remote access to a consistent desktop environment from any device.

## Wi-Fi Devices Replacing Network Cables
- Wi-Fi devices use radio frequency transmitters and receivers to send and receive data wirelessly, eliminating the need for physical Ethernet cables.

## Wireless Access Points (WAPs)
- Hardware devices that provide wireless access to a wired network by broadcasting Wi-Fi signals and connecting client devices.

### Wi-Fi Signal Transmission
- Wi-Fi signals are transmitted as radio waves through the air, making them vulnerable to interception unless secured with encryption protocols.

# Wi-Fi Security Protocols

## WEP (Wired Equivalent Privacy)
- The original Wi-Fi security protocol, now considered insecure due to weak encryption and easily cracked keys.

## WPA (Wi-Fi Protected Access)
- Improved over WEP with TKIP encryption but still has known vulnerabilities and is considered outdated.

## WPA2
- Uses stronger AES encryption with CCM (Counter with CBC-MAC) for data confidentiality and integrity; widely used and generally secure for most environments.

# Network Security

## WPA3
- The latest Wi-Fi security standard that uses CCMP (AES) and SAE (Simultaneous Authentication of Equals) for encryption.
- Provides robust protection against offline attacks.

## Pre-Shared Keys (PSK)
- A shared network password used by all users to connect to a Wi-Fi network.
- Easy to implement but less secure in shared environments.

## Enterprise Authentication
- Authenticates each user individually using credentials such as usernames and passwords.
- Typically involves a centralized authentication server like RADIUS.

## EAP (Extensible Authentication Protocol)
- A flexible framework for authentication in wireless and point-to-point connections.
- Supports various methods of credential exchange.

## EAP Variants
- **LEAP (Lightweight EAP)**: Developed by Cisco, uses MS-CHAP for authentication but is now considered insecure due to vulnerabilities.
- **PEAP (Protected EAP)**: Encapsulates EAP in a TLS tunnel to provide confidentiality and protect against eavesdropping.
- **EAP-TLS**: A highly secure method requiring both server and client-side digital certificates for mutual authentication and encrypted communication.
- **EAP-TTLS**: Wraps EAP within a secure TLS tunnel, requiring only a server-side certificate, making it easier to deploy with strong protection.
- **EAP-FAST**: A Cisco-developed protocol for fast and secure re-authentication using Protected Access Credentials (PACs), suited for enterprise environments.
- **EAP-MD5**: A basic method using MD5 hashing for authentication but is insecure due to vulnerabilities in the MD5 algorithm.

## Captive Portals
- A web-based authentication method used in public Wi-Fi networks that redirects users to a login or acceptance page before granting internet access.

# Authentication and Authorization Protocols

## RADIUS
- A centralized Authentication, Authorization, and Accounting (AAA) protocol used to manage user access and allow network access.

## RADIUS Server
- The system responsible for processing and verifying authentication requests, enforcing policies, and logging access attempts.

## RADIUS Client
- A network device like a switch, router, or wireless access point that sends user authentication requests to a RADIUS server.

# Wireless Networking

## Omnidirectional Antennas
- Antennas that radiate wireless signals equally in all horizontal directions, providing broad coverage suitable for both indoor and outdoor use.

# Wireless Security Tools

## Aircrack-ng
- A wireless network security tool used to assess Wi-Fi network security by capturing and analyzing packets to crack WEP and WPA/WPA2-PSK keys.

# Software Development and Security

## Code Security Tests
- Tests that use technology to detect security flaws in code to ensure software security against potential threats.

## Static Code Tests
- Automated techniques to analyze code for errors and security vulnerabilities without executing the code.

## Package Monitoring
- The process of tracking third-party code to ensure it doesn’t introduce vulnerabilities or licensing issues into a project.

# Cybersecurity Concepts

## Threat Intelligence
- Activities aimed at understanding changes in the cybersecurity threat landscape and adapting security controls accordingly.

## Open-Source Intelligence (OSINT)
- Information collected from publicly available sources used in an intelligence context.

## Email Address Harvesting
- The process of collecting email addresses, often for malicious purposes like spam or phishing, through methods such as scraping websites.

## Evaluating Threat Intelligence Sources
- **Timeliness**: Prompt delivery of information.
- **Accuracy**: Ensuring correct data.
- **Reliability**: Consistency of the data provider.

## Information Sharing and Analysis Centers (ISACs)
- Groups where different categories of companies share threat intelligence with each other.

## Threat Hunting
- A systematic approach to detecting indicators of compromise on networks using expertise and analytical techniques.

### Process of Threat Hunting
- Establish a hypothesis, identify associated indicators of compromise, and proceed to the incident response phase.

# Vulnerability Management

## Vulnerability Patching Process
- A company learns of a vulnerability, analyzes it, and develops a patch to address the issue.

## Window of Vulnerability
- The time period between the discovery of a vulnerability and the moment it is patched.

## Responsible Disclosure
- Informing the vendor of a vulnerability first and giving them a deadline before public disclosure.

# Log Management

## Syslog
- A standard protocol for sending system log messages to a central server.

### Components of a Syslog Message
- **Header**: Includes timestamp and source information.
- **Facility**: Identifies the source of the log.
- **Severity**: Indicates the urgency or importance of the message.
- **Message Content**: The details of the log entry.

## Syslog-ng
- A modern Syslog implementation supporting filtering, classification, and storage enhancements.

## Rsyslog
- An enhanced Syslog with better performance, filtering, and encryption support.

## journalctl
- A command-line utility to view logs managed by systemd’s journal.

## NXLog
- A universal log collector supporting multiple log formats and forwarding capabilities.

# Security Information and Event Management (SIEM)

## SIEM Functions with Logs
- Centralizes log collection and uses analytics to detect security events.

## SIEM Dashboards
- Provides real-time alerts, trends, and visual summaries of log data.

# Security Orchestration, Automation, and Response (SOAR)

## SOAR
- A platform that automates and coordinates security workflows.

## SOAR Playbooks
- Predefined workflows that guide automated incident response.

## SOAR Runbooks
- Executable steps that automate responses to specific security events.

# User and Entity Behavior Analytics (UEBA)

## UEBA
- Uses machine learning to detect anomalous behavior patterns and detect potential security threats.

# Automatable Processes

## Requirements for Automatable Processes
- They must be repeatable and not require human interaction.

# Professional Notes on Security and Authentication Concepts

## TLS and SSL
- **TLS**: A protocol for secure communication over a network that uses other encryption algorithms. It involves a handshake where a client and server agree on encryption, verify certificates, and exchange session keys.
- **SSL**: Secure Sockets Layer, the predecessor to TLS, is now considered insecure.
- **OpenSSL**: An open-source implementation of the TLS/SSL protocol suite.
- **SSL Inspection**: A technique where encrypted traffic is decrypted, inspected for threats, and then re-encrypted before being forwarded.
- **Ephemeral Keys**: Temporary session keys used for a single TLS session to enhance security.

## IPsec and Protocols
- **IPsec**: A suite of protocols that secure IP traffic by authenticating and encrypting packets.
- **ESP (Encapsulating Security Payload)**: Encrypts packet data in IPsec to ensure confidentiality.
- **AH (Authentication Header)**: Ensures data integrity and authentication without encryption in IPsec.
- **SA (Security Association)**: A set of policies and keys used to authenticate and encrypt IPsec traffic.
- **HTTPS**: Secure version of HTTP that uses TLS to encrypt data transmitted over the web.

## Network and File Transfer Protocols
- **Telnet**: An unencrypted protocol for remote command-line access.
- **SSH (Secure Shell)**: An encrypted protocol that replaces Telnet for secure remote access.
- **FTP**: A file transfer protocol that is unencrypted by default.
- **FTPS**: FTP with added SSL/TLS encryption for secure file transfers.
- **SFTP**: Secure File Transfer Protocol, which uses SSH to encrypt file transfers.
- **SCP (Secure Copy Protocol)**: A command-line tool for secure file transfer using SSH.
- **TFTP (Trivial File Transfer Protocol)**: A lightweight, unsecured version of FTP used in limited scenarios.
- **RTP**: Real-Time Protocol used for delivering audio and video over IP networks.
- **SRTP**: Secure Real-Time Protocol, the encrypted version of RTP.
- **NTP**: Network Time Protocol used to synchronize system clocks.

## Email and Directory Protocols
- **POP3**: Post Office Protocol version 3 used to retrieve email, often over SSL/TLS on port 995.
- **IMAP**: Internet Message Access Protocol, used to retrieve email, typically over SSL/TLS on port 993.
- **SMTP**: Simple Mail Transfer Protocol, used to send email over SSL/TLS on port 465.
- **S/MIME**: Secure/Multipurpose Internet Mail Extensions, providing email encryption and digital signatures.
- **LDAP**: Lightweight Directory Access Protocol used to manage and access directory information.
- **LDAPS**: Secure version of LDAP, utilizing TLS for encryption (typically port 636).

## Authentication and Access Control
- **AAA**: Authentication, Authorization, and Accounting, which are core principles of access control.
- **IAM**: Identity and Access Management systems that manage user identities and access privileges.
- **Authentication Factors**: Something you know (password), have (device), are (biometric), or your location.
- **Authentication Errors**: False Acceptance Rate (FAR) and False Rejection Rate (FRR) are two key metrics for authentication accuracy.

## Biometric and Physical Authentication
- **Biometric Authentication**: Uses physical characteristics like fingerprints, face, iris, or voice patterns for identification.
- **Registration Process for Biometrics**: The process of capturing and securely storing a user’s biometric data for future comparison.
- **Smart Cards**: Authentication cards with embedded chips for secure login.
- **Magnetic Stripe Cards**: Authentication cards with data stored on a magnetic strip.

## Password and Key-Based Authentication
- **Security Key**: A physical device used in two-factor authentication for secure access.
- **Key-Based Authentication**: The use of a private key paired with a public key to prove identity.
- **Federated Identity Management**: A system that allows identity sharing across organizations for seamless access.
- **Single Sign-On (SSO)**: A system that allows a single login to provide access to multiple services.

## Access Control Models
- **Mandatory Access Control (MAC)**: Access to resources is controlled by the operating system based on classification labels and clearance levels.
- **SELinux**: Security-Enhanced Linux, which implements MAC to enforce strict access controls.
- **Discretionary Access Control (DAC)**: Access control where the resource owner determines who can access resources.
- **Role-Based Access Control (RBAC)**: Permissions are granted based on the user’s role within an organization.
- **Attribute-Based Access Control (ABAC)**: Access control based on policies evaluating attributes of the user, resource, and environment.

## Trust Models and Kerberos
- **Kerberos**: A ticket-based authentication protocol using a centralized authentication server.
- **NTLM Authentication**: Uses a challenge-response protocol based on password hashes.
- **SAML**: Security Assertion Markup Language, an XML-based standard for Single Sign-On (SSO).
- **OAuth and OpenID Connect**: OAuth is used for authorizing third-party apps without sharing passwords, while OpenID Connect builds on OAuth for authentication.

## Security Policies and Practices
- **Privilege Creep**: Accumulation of unnecessary permissions over time due to job changes.
- **Default Deny Principle**: A security principle where all access is denied by default unless explicitly authorized.
- **Password Vaulting**: The practice of securely storing passwords in an encrypted vault.
- **Command Proxying**: A technique where user commands are routed through a controlled intermediary to monitor and restrict actions in real time.

## Emergency and Temporary Access
- **Ephemeral Accounts**: Temporary accounts created for short-term use, typically for contractors or emergency access, which are automatically deleted after use.
- **Emergency Access Workflow**: A predefined process to grant temporary elevated access during a crisis while maintaining security.

## Trust Models in Domain Relationships
- **One-Way Trust**: One domain trusts another, but the reverse is not true.
- **Two-Way Trust**: Both domains trust each other.
- **Transitive Trust**: Trust extends through chains of relationships.

# Incident Response and Digital Forensics Notes

## Account Provisioning and Deprovisioning
- **Provisioning** refers to creating and configuring new accounts, ensuring they have the appropriate access levels and permissions.
- **Deprovisioning** involves disabling or deleting accounts when they are no longer needed, such as when an employee leaves or changes roles.

## User Exit Workflow
### Routine Workflow
- **Deactivate Accounts**: Disable or lock accounts to prevent access.
- **Revoke Access**: Ensure all system access is terminated.
- **Recover Devices**: Retrieve any company-owned devices from the user.
- **Review Data Ownership/Permissions**: Verify who owns data and adjust permissions as necessary.

### Emergency Workflow
- **Immediate Account Lockout**: Quickly lock the user's account to prevent unauthorized access.
- **Network Access Termination**: Disconnect the user’s access to the organization's network.
- **Alert Security Teams**: Notify relevant teams to prevent potential insider threats.

## Incident Response Plan
- **Elements**: 
  - Statement of purpose
  - Response strategies and goals
  - Nature of response approach
  - Communication plans
  - Senior leadership approval
- **NIST SP 800-61**: A NIST special publication offering guidance for creating and maintaining an effective incident response plan.

## First Responder Priorities
- **Containment**: The first priority during an incident is to contain the damage by isolating affected systems and preventing further spread of the incident.

### Incident Handling Steps
1. **Containment**: Isolate affected systems.
2. **Escalation and Notification**: Inform relevant personnel and stakeholders based on the severity of the incident.
3. **Triage**: Categorize incidents based on their impact level: low, medium, or high. This helps prioritize the response.

### Incident Impact Levels
- **Low-Impact**: Minor issues with little to no business disruption.
- **Medium-Impact**: Affects limited users or services, requiring prompt attention.
- **High-Impact**: Causes widespread damage or compromise requiring immediate and extensive response.

## Incident Containment Strategy
Key considerations for containment include:
- Damage potential
- Evidence preservation
- Service availability
- Resource requirements
- Expected effectiveness of the containment solution
- Time frame for implementing the solution

### Containment Methods
- **Segmentation**: Isolate affected areas from the rest of the network.
- **Isolation**: Disconnect affected systems from the network.
- **Removal**: Take compromised devices offline entirely.

## Incident Eradication and Recovery
- **Eradication Goal**: Completely remove the root cause and all traces of the incident from the environment.
- **Recovery Goal**: Restore normal business operations, ensuring that the threat is neutralized and systems are secure.
- **Rebuilding Systems**: Rebuild or reimage compromised systems to eliminate lingering threats such as malware or backdoors.

## Sanitization Techniques
- **Clearing**: Overwriting data to prevent casual recovery.
- **Purging**: More rigorous data removal methods.
- **Destroying**: Physically destroying media to ensure data cannot be recovered.

## Post-Incident Activities
- Conduct a **lessons learned** session to improve future response strategies.
- Retain **evidence** for legal or audit purposes.
- Generate **Indicators of Compromise (IOCs)** for future detection and response.
- Perform a **root cause analysis** to determine how the incident occurred and how to prevent it in the future.

## Digital Forensics
- **Definition**: The process of collecting, preserving, analyzing, and presenting digital evidence in a way that is legally admissible.

### Order of Volatility
The order in which evidence should be collected based on its likelihood to be lost over time:
1. **Network Traffic**
2. **Memory Contents (RAM)**
3. **System/Process Data**
4. **Temporary Files**
5. **Logs**
6. **Archived Records**

### Ensuring Evidence Integrity
- Forensic investigators ensure evidence hasn’t been altered by generating a **hash** of the original data and comparing it to the hash of the copied data.

### Metadata
- Metadata provides valuable information, such as:
  - File creation date
  - Last modified date
  - Author
  - Access history
This data is critical for forensic analysis.

### Evidence Log Events
- **Evidence Log**: Documents the timeline and actions related to handling digital evidence from collection to analysis.
- **Evidence Log Entry Details**: Must include the handler’s name, date/time, reason for access, and the action taken to maintain the integrity of the chain of custody.

## System Log Management
- System administrators must **suspend automatic deletion of relevant logs** to preserve evidence for investigations or legal proceedings, preventing the loss of critical forensic data.
