# Certificate Transparency (CT) Logs
## Overview
Certificate Transparency (CT) logs are **public, append-only ledgers** that record the issuance of **SSL/TLS certificates**. Certificate Authorities (CAs) are required to submit all issued certificates to these logs, which are maintained by independent organizations.
## Purpose & Benefits
- **Early Detection of Rogue Certificates**: Helps identify misissued or fraudulent certificates that could enable spoofing or MITM attacks.
- **Accountability for CAs**: All issued certificates are publicly visible, holding CAs accountable for improper issuance.
- **Strengthens the Web PKI**: Enables public oversight of certificate issuance, enhancing trust in encrypted web communication.

---
## CT Logs for Reconnaissance
### Why CT Logs Matter in Pentesting
- **Subdomain Enumeration**: CT logs reveal subdomains by listing the `Subject Alternative Name (SAN)` fields in issued certificates.
- **Bypasses Wordlist Limitations**: Unlike brute-forcing, CT logs offer a comprehensive view of both active and historical subdomains.
- **Exposure of Legacy Systems**: Older or expired certificates may point to forgotten or vulnerable infrastructure.

---
## Tools to Search CT Logs

| Tool       | Key Features                                                      | Pros                              | Cons                           |
| ---------- | ----------------------------------------------------------------- | --------------------------------- | ------------------------------ |
| **crt.sh** | Web interface, SAN listing, certificate details                   | Free, no login, simple searches   | Limited filtering & automation |
| **Censys** | Search by domain, IP, certificate attributes, supports API access | Advanced filtering, in-depth data | Requires registration          |

---
### Automating CT Log Searches with crt.sh
To programmatically extract subdomains using `crt.sh`:
```bash
curl -s "https://crt.sh/?q=facebook.com&output=json" \
| jq -r '.[]
  | select(.name_value | contains("dev"))
  | .name_value' \
| sort -u
```
- `curl -s`: Retrieves the JSON output for `facebook.com`.
- `jq`: Filters for entries where the `name_value` includes "dev".
- `sort -u`: Sorts results and removes duplicates.

**Example Output**:
```
*.dev.facebook.com
dev.facebook.com
secure.dev.facebook.com
newdev.facebook.com
```
