# DNS
DNS (Domain Name System) translates human-readable domain names (e.g., `www.example.com`) into machine-readable IP addresses (e.g., `192.0.2.1`). It's a hierarchical and distributed naming system essential for internet communication.

---
## How DNS Works (Query Flow)
1. **Local Cache Check:** The client checks its local cache for a previously resolved IP.
2. **DNS Resolver Query:** If no cache hit, it queries a DNS resolver (usually from the ISP).
3. **Root Server Referral:** Resolver asks a root name server, which refers it to the appropriate TLD name server.
4. **TLD Server Referral:** The TLD server refers to the domain’s authoritative name server.
5. **Authoritative Response:** The authoritative server returns the IP address.
6. **Resolver Cache + Response:** Resolver caches the response and sends it to the client.
7. **Client Connects:** The client uses the IP to reach the web server.

---
## Hosts File
Used to override DNS resolution locally.
- **Windows:** `C:\Windows\System32\drivers\etc\hosts`
- **Linux/macOS:** `/etc/hosts`
- **Format:** `<IP> <Hostname> [Alias]`
- **Use Cases:** Development, testing, blocking domains

**Examples:**
```text
127.0.0.1   myapp.local
192.168.1.20 testserver.local
0.0.0.0   unwanted-site.com
```

---
## Key DNS Concepts
- **Domain Name:** Human-readable site address (e.g., `example.com`)
- **IP Address:** Numeric identifier (e.g., `192.0.2.1`)
- **DNS Resolver:** Translates domain names to IPs (e.g., 8.8.8.8)
- **Root Name Server:** Directs queries to TLD servers
- **TLD Name Server:** Manages `.com`, `.org`, etc.
- **Authoritative Name Server:** Provides final IP mapping for domain

---
## Common DNS Record Types

|Type|Purpose|Example|
|---|---|---|
|A|Maps hostname to IPv4|`www.example.com. IN A 192.0.2.1`|
|AAAA|Maps hostname to IPv6|`www.example.com. IN AAAA 2001:db8::1`|
|CNAME|Alias for another hostname|`blog.example.com. IN CNAME web.example.com.`|
|MX|Mail server for domain|`example.com. IN MX 10 mail.example.com.`|
|NS|Authoritative name servers|`example.com. IN NS ns1.example.com.`|
|TXT|Arbitrary text (e.g., SPF/DKIM/verification)|`example.com. IN TXT "v=spf1 mx -all"`|
|SOA|Admin info for DNS zone|Includes serial, refresh, retry, expire, TTL|
|SRV|Service-specific record|`_sip._udp.example.com. IN SRV 10 5 5060 sip.example.com.`|
|PTR|Reverse lookup (IP to domain)|`1.2.0.192.in-addr.arpa. IN PTR www.example.com.`|

**Note:** `IN` denotes the Internet class of DNS records.

---
## DNS Zone Files
Zone files define records for a domain and reside on authoritative DNS servers.

**Example:**
```zone
$TTL 3600
@ IN SOA ns1.example.com. admin.example.com. (
   2024060401 ; Serial
   3600       ; Refresh
   900        ; Retry
   604800     ; Expire
   86400 )    ; Minimum TTL

@    IN NS     ns1.example.com.
@    IN NS     ns2.example.com.
@    IN MX 10  mail.example.com.
www  IN A      192.0.2.1
ftp  IN CNAME  www.example.com.
```
