
# DNSEnum

```bash
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -r
```

**Options Explained:**
- `--enum`: Enables full DNS enumeration (zone transfer, subdomain brute-force, etc.) and specifies the target domain to enumerate
- `-f`: Specifies wordlist (e.g., from SecLists)
- `-r`: Recursively brute-forces discovered subdomains

**Output Example:**
```
support.inlanefreight.com.    300   IN   A   134.209.24.248
mail.inlanefreight.com.       300   IN   A   134.209.24.248
```

---
