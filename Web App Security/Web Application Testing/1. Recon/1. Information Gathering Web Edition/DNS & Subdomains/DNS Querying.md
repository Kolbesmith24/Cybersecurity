
# DNS Recon

## **Core Tools**

| Tool             | Purpose                                                                |
| ---------------- | ---------------------------------------------------------------------- |
| **dig**          | Versatile DNS queries (A, MX, NS, TXT, etc.); supports zone transfers. |
| **nslookup**     | Simple tool for A, AAAA, MX records.                                   |
| **host**         | Quick lookups with concise output.                                     |
| **dnsenum**      | Automated subdomain enumeration, brute force, zone transfers.          |
| **fierce**       | Recursive subdomain enumeration, wildcard detection.                   |
| **dnsrecon**     | Combines multiple techniques, supports various output formats.         |
| **theHarvester** | Collects DNS info (e.g., emails) from OSINT sources.                   |

## **Essential `dig` Commands**

|Command|Description|
|---|---|
|`dig domain.com`|A record lookup (default).|
|`dig domain.com MX`|Find mail servers.|
|`dig domain.com NS`|List authoritative name servers.|
|`dig domain.com TXT`|Retrieve TXT records.|
|`dig domain.com CNAME`|Canonical name record.|
|`dig domain.com SOA`|Start of authority info.|
|`dig +trace domain.com`|Trace DNS resolution path.|
|`dig -x IP`|Reverse DNS lookup.|
|`dig +short domain.com`|Return only the answer (clean output).|
|`dig +noall +answer domain.com`|Show only the answer section.|
|`dig @nameserver domain.com`|Query a specific DNS server.|
|`dig domain.com ANY`|All record types (often blocked).|

> ⚠️ **Note**: Excessive DNS queries can trigger rate limiting or blocks. Always get permission before scanning targets.

# Key Tools for Subdomains

|Tool|Notes|
|---|---|
|**dnsenum**|DNS recon tool; supports brute-forcing, zone transfers, WHOIS lookups|
|**fierce**|Recursive subdomain discovery; handles wildcard DNS|
|**dnsrecon**|Broad DNS recon with customizable output|
|**amass**|Integrates active/passive discovery; widely used|
|**assetfinder**|Lightweight, quick subdomain finder|
|**puredns**|High-performance resolver; good for large wordlists|

---
