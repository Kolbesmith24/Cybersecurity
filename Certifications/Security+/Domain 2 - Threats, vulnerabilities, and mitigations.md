# Cybersecurity Risk Types and Threats

## Risk Types

- **Strategic Risk**: Risks that jeopardize the ability to meet organizational goals and objectives.
- **Operational Risk**: Risks affecting the ability to carry out day-to-day activities.
- **Compliance Risk**: Risks related to potential violations of laws or regulations.

## Product Lifecycle Terms

- **End of Sale**: A product is no longer available for sale but will continue to be supported.
- **End of Support**: Vendor stops providing support for the product.
- **End of Life**: Vendor ceases all support, updates, and services for the product.

## Security Issues in IT Systems

- **Default Configurations**: Insecure configurations such as misconfigured firewalls, open permissions, guest accounts, and default passwords.
- **Weak Cipher Suites or Cryptographic Protocols**: Outdated or insecure encryption methods for data protection.
- **Poor Key Management**: Insecure handling, storage, distribution, or lifecycle management of cryptographic keys.
- **Poor Certificate Management**: Mismanagement of certificates leading to expiration, misuse, or compromise.
- **Patch Management**: Updating and applying patches to operating systems, applications, and firmware to address vulnerabilities.

## Principles and Architecture

- **Principle of Least Privilege**: Granting users, systems, or applications the minimum access level necessary to perform their tasks.
- **IT Architecture**: The processes and practices used to design and structure IT systems.
- **Architectural Vulnerabilities**: Flaws or weaknesses in the overall design or structure of a system.
- **System Sprawl**: The tendency for old devices to remain connected to a network while new devices are added, creating security vulnerabilities.

## Malware and Prevention

- **Malware**: Malicious software consisting of two components:
  - Propagation Mechanism: How it spreads.
  - Payload: Malicious action performed.
- **Virus Prevention**: Spreads from system to system with user interaction; prevention involves teaching proper security practices.
- **Worms Prevention**: Spreads system-to-system independently; prevention involves keeping systems up to date.
- **Trojans Prevention**: Appears as legitimate software with a hidden malicious payload; prevent via application control.
- **RATs (Remote Access Trojans)**: Provide backdoors for hackers to remotely access a system.
- **Spyware**: Malware that secretly gathers information, such as keystroke logging and monitoring web browsing.
- **Bloatware**: Unwanted software that comes pre-installed on devices.
- **Ransomware**: Blocks access to a system or files until a ransom is paid.
- **Crypto Malware**: Uses computer resources to mine cryptocurrency.
- **Preventing Malware**: Involves using anti-malware software, educating users, and updating systems.

## Types of Malware and Attack Mechanisms

- **Backdoors**: Mechanisms that allow unauthorized access to systems.
- **Logic Bombs**: Malware that triggers a payload upon reaching a specific condition.
- **Rootkits**: Malware designed to escalate user privileges.
- **User Mode vs. Kernel Mode**: User mode operates with regular permissions, while kernel mode operates with root-level privileges.
- **Fileless Viruses**: Malware that avoids detection by not writing data to disk but operates through memory.
- **Botnets**: Networks of infected machines used for activities like spam, DDoS attacks, and cryptocurrency mining.
- **Botnet Command and Control**: Communication methods for botnets, including relay orders, Internet Relay Chat (IRC), and peer-to-peer (P2P) communication.

## Programming and Scripting

- **Shell Scripts**: Scripts run at the command line.
- **Application Scripts**: Scripts executed within application software.
- **Programming Languages**: Code executed through a programming language.

## Threat Actors and Attacks

- **Hacktivists**: Individuals who use hacking tools for political or social purposes.
- **Nation-State Hacking Actors**: Highly skilled attackers, often trained by government agencies.
- **Preventing Insider Threats**: Background checks and applying the principle of least privilege.
- **Attack Vectors**: Paths used by hackers to gain unauthorized access, such as phishing messages, social media, or USB devices.
- **APTs (Advanced Persistent Threats)**: Long-term, skilled attacks, often sponsored by governments, exploiting zero-day vulnerabilities.
- **Defending Against APTs**: Strong system security measures and encryption.
- **Spear Phishing**: Targeted phishing with specific payloads based on the attack's target.
- **Whaling Attacks**: Phishing targeting executives to steal information or gain access.
- **Pharming Attacks**: Using fake websites to steal user information.
- **Vishing**: Voice phishing to steal sensitive information.
- **Smishing and SPIM**: Phishing through SMS and iMessage.
- **Spoofing**: Faking an identity to deceive others.
- **Pretexting Attacks**: Attackers pretend to be legitimate entities to gain unauthorized access.
- **Watering Hole Attacks**: Techniques used to lure unsuspecting users to malicious sites.
- **Shoulder Surfing**: Observing someone’s screen to gain sensitive information.
- **BEC (Business Email Compromise)**: Targets businesses or individuals through phishing attacks.
- **Vendor Email Compromise**: Attackers monitor vendor communication and create fake invoices to steal payments.

## Information Manipulation and Security

- **Propaganda**: The spread of false information to manipulate opinions.
- **Deepfakes**: AI-generated false images, audio, or videos to mislead people.
- **SEO (Search Engine Optimization)**: Exploits search engine results to manipulate information visibility.

## File Security

- **/etc/passwd File**: A Linux file containing user information and password hashes. To secure it, avoid storing password hashes here.
- **/etc/shadow File**: A more secure version of the passwd file for storing password hashes.

## Password and Authentication Attacks

- **Hash Function Criteria**:
  - Different outputs for each input.
  - Difficult to reverse the output to obtain the original input.
  - Computationally hard to find collisions (two different inputs producing the same output).
- **Dictionary Attacks**: Using a list of common words as potential passwords.
- **Hybrid Attacks**: Variations of dictionary attacks with added modifications.
- **Rainbow Table Attacks**: Precomputing password hashes and comparing them with others to uncover passwords.
- **Password Spraying**: Trying common passwords across multiple accounts to gain unauthorized access.
# Cybersecurity Topics

## Credential Stuffing
- Exploits reused passwords to login to other sites.

## Cross-Site Scripting (XSS)
- Cross-site scripting occurs when an attacker embeds malicious scripts in a third-party website.

## Cross-Site Request Forgery (CSRF / XSRF)
- Cross-site request forgery uses embedded malicious code on one site to trick the browser into sending malicious requests to another site (where the user is already logged in) without the user's knowledge and with their account.

## Server-Side Request Forgery (SSRF)
- Targets servers into retrieving malicious data from what it believes to be a trusted source.

## Buffers
- Areas of memory set aside to store user-supplied content.

## Buffer Overflow Attacks
- Exploits occur when input larger than the buffer is provided, causing memory corruption or code execution.

## Cookies
- Track user activity across the web, often used for web server logins to reference earlier, successful logins.

### Cookie Guessing (AuthCookie Attack)
1. Login to a site with user1 and take note of the authcookie.
2. Logout and login as user2, then take note of the authcookie.
3. Compare the authcookies for user1 and user2, and find patterns between them.
4. Use the pattern to generate an authcookie for other users, such as admin, then intercept the login request to authenticate as admin.

### Session Replay Attack
- Attacker eavesdrops on a user session and steals their cookie to log in as them.

### Secure Cookies
- Cookies should always be sent over encrypted connections to prevent eavesdropping.

## Code Execution Attack
- Exploits vulnerabilities to run commands on the system.

### Remote Code Execution (RCE) / Arbitrary Code Execution (ACE)
- RCE allows attackers to run commands over a network connection.
- ACE allows attackers to run commands locally on the system.

### Preventing Code Execution Attacks
- Limit application and administrative privileges.
- Patch systems and applications to remove vulnerabilities.

### Mitigating Privilege Escalation
- Perform input validation, patch systems, and enforce the least privilege principle.

## OWASP Top Ten
- A list of common web security vulnerabilities.
  
### Broken Access Control
- Developers fail to check on the backend whether a user is authorized to access specific functions.

### Insecure Direct Object References (IDOR)
- Occurs when a developer exposes application details without proper security checks, such as changing the ID number in a URL to access unauthorized resources.

### Cryptographic Failures
- Sensitive information is accidentally exposed due to poor cryptographic implementation, such as failing to implement HTTPS properly.

### Insecure Design
- Security issues arise when organizations fail to implement proper security practices.

### Security Misconfigurations
- Errors in any security settings from web servers, databases, application services, routers, firewalls, etc.

### Application Hardening
- Practices to secure applications include proper authentication, encrypting sensitive data, validating user input, remediating known exploits, and applying security patches immediately.

## Directory Traversal Attacks
- Occurs when an attacker moves up and down an application's directories to search for insecure files.

### Defending against Directory Traversal Attacks
- Methods include input validation and file access controls.

## Race Conditions
- A vulnerability that occurs when the proper function of a security control depends on the timing of an action.

### Time of Check/Time of Use Vulnerability
- A vulnerability where software checks if activity is authorized, but a delay before the action allows multiple users to exploit the resource (e.g., two users withdrawing money from an account with insufficient funds).

## Keyspace
- The set of all possible encryption keys that can be used with an application.

## Frequency Analysis
- An attacker analyzes the patterns of letters in cipher-text to break the encryption.

## Birthday Attack
- Exploits collisions in hash functions where two different inputs produce the same hash output.

## Downgrade Attack
- Tricking two parties into downgrading encryption so the attacker can intercept and view the message content.

## Smurf Attack
- An attacker sends ICMP echo requests to broadcast addresses of third-party servers with a forged source address, causing the servers to send replies to the victim.

## Amplified Attack
- An attacker sends small requests that receive large responses, amplifying the attack's impact.

## Denial of Service (DoS) Attacks Target
- DoS attacks target bandwidth, application, and operational technologies (OTs).

## Eavesdropping Attack
- Attackers listen to information being sent through a communication path.

## Man-in-the-Middle (MiTM) Attack
- An attacker sits between the victim and the server, intercepting and potentially altering requests.

### Man-in-the-Browser Attack
- A type of MiTM attack where the attack occurs through a web browser, such as via a malicious browser extension.

### Replay Attack
- An attacker uses previously authenticated data (e.g., authentication token) to communicate with the server, pretending to be the victim. This can be prevented with session tokens and timestamps.

## SSL Stripping
- Tricking browsers into using unencrypted communications.

## Domain Name System (DNS)
- The system that translates IP addresses into hostnames or server names.

### DNS Poisoning
- An attacker submits incorrect DNS records to redirect traffic to their web server.

### Typosquatting
- Exploiting common typing errors in domain names to redirect users to an attacker's site.

### Domain Hijacking
- An attacker takes over domain registration from the rightful owner.

### URL Redirection
- Attacker places content on a legitimate site to redirect users to a malicious site.

### Domain Reputation
- Determines whether traffic is coming from a trusted domain.

## Wireless Network Security

### WEP (Wired Equivalent Privacy)
- Attacker captures initialization vectors to help establish a connection.

### WPA (Wi-Fi Protected Access)
- Adds TKIP, which changes keys frequently, but is now insecure. Use WPA2 or WPA3 for stronger encryption.

### WPS (Wi-Fi Protected Setup)
- Allows quick setup for new devices using an 8-digit pin or pressing two buttons.

### WPS Attacks
- Attackers can determine the WPS pin and obtain the encryption key.

### Jamming & Interfering Attack
- A DoS attack where the attacker broadcasts a strong signal to overpower a wireless access point.

### Wardriving and Warflying Attacks
- Wardriving: Walking or driving around to gather information from wireless networks.
- Warflying: Similar to wardriving, but conducted using a drone.

### Rogue Access Points
- Unauthorized wireless access points connected to a network that bypass authentication.

### Evil Twin Attacks
- An attacker sets up a fake access point that replicates a legitimate access point’s SSID.

### Disassociation Attacks
- Performed through deauthentication frames, which kick a user off the network and force re-authentication.

## Bluetooth Security

### Bluejacking
- Attackers use Bluetooth to send spam messages.

### Bluesnarfing
- Attackers force pairing to steal information from a device.

### Improving Bluetooth Security
- Recommendations include disabling Bluetooth when not in use and applying updates consistently.

## RFID Security Concerns
- RFID chips can be scanned and duplicated by attackers, leading to data theft or unauthorized access.

### Public Attack Indicators
- Indications of an attack shared among agencies to help organizations identify attacks.

## NTLM Authentication Protocol
- An outdated protocol used by Microsoft systems, now considered insecure.

### NTLM Vulnerabilities
- NTLM is vulnerable to pass-the-hash, replay, and brute-force attacks.

### Modern Replacement for NTLM
- Kerberos is the modern, more secure alternative to NTLM.

## On-Path Attack
- An on-path attack (formerly known as a MiTM attack) occurs when an attacker intercepts and potentially alters communications between two parties without their knowledge.

## Man-in-the-Browser Attack Indicators
- Unexpected redirects, new or spoofed pop-ups asking for credentials, browser extensions or processes that shouldn’t be there, and modified transaction details without user input.
