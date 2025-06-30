# WHOIS Overview
**WHOIS** is a protocol used to query databases storing information about registered internet resources, especially domain names. It provides critical details such as ownership, registration, contact information, and DNS infrastructure. WHOIS is a foundational tool in reconnaissance for cybersecurity assessments.

---
## Key WHOIS Fields
Typical WHOIS records include:
- **Domain Name**
- **Registrar**: Where the domain was registered
- **Registrant/Administrative/Technical Contact**: Individuals or organizations responsible for the domain
- **Creation and Expiration Dates**
- **Name Servers**: Translate the domain into IP addresses

---
## Importance in Reconnaissance
WHOIS supports penetration testers by revealing:
- **Personnel Info**: Names and contact details usable for social engineering
- **Network Clues**: Name servers and hosting info may indicate misconfigurations
- **Historical Changes**: Older records (e.g., via WhoisFreaks) show shifts in ownership or infrastructure over time

---
## Practical Scenarios
**1. Phishing Investigation**  
WHOIS can identify suspicious domains (e.g., recent registration, privacy-protected registrants, bulletproof hosting), helping analysts flag phishing campaigns.
**2. Malware Analysis**  
Reveals potentially malicious infrastructure (e.g., anonymous registration, suspicious registrar) tied to C2 servers.
**3. Threat Intelligence**  
Enables profiling of threat actors by correlating patterns across multiple domains (e.g., shared name servers, clustered registration dates, repeated takedowns).

---
### Using WHOIS
To install on Linux:
```bash
sudo apt update
sudo apt install whois -y
```

To perform a lookup:
```bash
whois example.com
```

**Example Output – facebook.com**:
- **Registrar**: RegistrarSafe, LLC
- **Created**: 1997-03-29
- **Expires**: 2033-03-30
- **Registrant**: Meta Platforms, Inc.
- **Domain Status**: Protected from deletion/transfer/updates
- **Name Servers**: A/B/C/D.NS.FACEBOOK.COM

This information confirms the legitimacy, maturity, and security posture of the domain.
