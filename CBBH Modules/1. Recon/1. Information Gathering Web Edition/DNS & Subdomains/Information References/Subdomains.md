# Subdomains
**Why Subdomains Matter in Recon:**
- Subdomains often reveal:
    - **Development/Staging environments** with weak security
    - **Hidden login/admin portals**
    - **Legacy applications** with outdated software
    - **Exposed sensitive data** (e.g., documents, configs)

**Subdomain Enumeration Techniques:**
## 1. **Active Enumeration**
- Directly queries DNS servers
- **Brute-force** common subdomain names using tools:
    - `dnsenum`
    - `ffuf`
    - `gobuster`
- **Zone transfers** may reveal full DNS data but are rarely misconfigured today
## 2. **Passive Enumeration**
- Uses external sources, without querying target:
    - **Certificate Transparency Logs**: look at SAN fields
    - **Search Engines**: e.g., `site:*.example.com`
    - **Public DNS databases and tools**: collect aggregated data