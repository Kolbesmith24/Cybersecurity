
# DNS Zone Transfers (AXFR)
**Purpose**  
A DNS zone transfer is used to replicate DNS records from a **primary** to a **secondary** name server. If misconfigured, it can be exploited to extract a full list of a domain’s DNS records, including subdomains and internal infrastructure.

---

## **What Happens in a Zone Transfer (AXFR)**
1. **Request Initiation**: Secondary server sends a zone transfer request (AXFR) to the primary DNS server.
2. **SOA Record**: Primary responds with the Start of Authority (SOA) record (includes serial, refresh, retry, etc.).
3. **Record Transfer**: All zone records (A, AAAA, MX, NS, CNAME, TXT, SRV, etc.) are transferred.
4. **Completion Signal**: Primary signals end of transfer.
5. **ACK**: Secondary server sends acknowledgment.

---
## **Why This Matters for Pentesting**
- **Misconfigurations**: If zone transfers are allowed to any client, attackers can download the entire DNS zone.
- **Recon Value**:
    - Full list of **subdomains** (including dev, staging, admin panels)
    - **IP addresses** of hosts/subdomains
    - **Mail servers** and **name servers**
    - Misc info (TXT, SRV, HINFO, etc.)

---
### **Real-World Impact**
- Reveals internal infrastructure not publicly exposed.
- Can be a critical starting point for deeper recon and attack surface mapping.
- Misconfigurations still exist due to outdated setups or human error.

---

## **How to Attempt a Zone Transfer (Linux/macOS)**

```bash
dig axfr <name_server> @<target_domain>
```

**Example** (demo domain):

```bash
dig axfr inlanefreight.htb @10.129.72.190
```

If successful, the result includes a full DNS dump:
- SOA
- A/AAAA/CNAME/MX/NS/TXT/SRV records
- Subdomain-IP mappings

---
## **Security Best Practices (Defensive)**
- Only allow AXFR from **authorized secondary DNS servers**.
- Apply **strict access control** for zone transfers on DNS servers.
- Regularly audit DNS configurations.
