# **Virtual Hosts (VHosts) in Web Servers**
## **Overview**
Web servers like Apache, Nginx, and IIS use **virtual hosting** to serve multiple domains or subdomains from the same server/IP. This is managed via the **HTTP Host header**, which informs the server which site to serve.
- **Subdomains**: e.g., `blog.example.com` – may have separate DNS records; used for service segmentation.
- **Virtual Hosts**: Server-level configurations for hosting different domains or subdomains on one server. Can operate with or without DNS records.

> **Accessing VHosts without DNS**: Modify your local `hosts` file to resolve domain names directly to an IP address (e.g., `10.10.10.10 dev.example.com`).

---
## **How VHosts Work**
1. Browser sends request with `Host` header (e.g., `Host: dev.example.com`).
2. Web server matches the `Host` header to the correct `<VirtualHost>` block.
3. Corresponding content is served from its `DocumentRoot`.
### **Example – Apache Name-Based VHosts**
```apache
<VirtualHost *:80>
    ServerName www.example1.com
    DocumentRoot /var/www/example1
</VirtualHost>
```

---

## **Types of Virtual Hosting**

|Type|Description|Pros|Cons|
|---|---|---|---|
|**Name-Based**|Uses `Host` header (most common). Multiple domains on one IP.|Cost-effective, flexible, easy to configure|May conflict with SSL/TLS (without SNI)|
|**IP-Based**|Each domain uses a unique IP address.|Works with all protocols, better isolation|Requires multiple IPs, less scalable|
|**Port-Based**|Multiple domains on different ports of the same IP (e.g., `:80`, `:8080`).|Useful when IPs are limited|Less intuitive for users, needs port in URL|

---
## **VHost Discovery (Fuzzing)**
Hidden or internal subdomains/VHosts may not appear in DNS. Use **Host header fuzzing** to discover them.
### **Common Tools**

|Tool|Description|
|---|---|
|**Gobuster**|Brute-forces Host headers. Supports custom wordlists, threading, filters|
|**ffuf**|Web fuzzer that supports Host header fuzzing with wordlist input|
|**Feroxbuster**|Fast, recursive, flexible fuzzer written in Rust|

---

## **Using Gobuster for VHost Discovery**
### **Preparation**
- **Target IP**: Use IP, not domain (to avoid DNS lookup interference).
- **Wordlist**: Use curated lists (e.g., `/usr/share/seclists/Discovery/DNS/`).
	- https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS
### **Command**
```bash
gobuster vhost -u http://<target_IP>:PORT -w <wordlist> --append-domain
```
- `-u`: Target URL (use IP address)
- `-w`: Path to wordlist
- `--append-domain`: Appends the base domain to each word (e.g., `dev.example.com`)
- `-t`: Increase threads (e.g., `-t 50`)
- `-k`: Ignore SSL/TLS errors
- `-o`: Save output to file

> ⚠️ `--append-domain` is required in newer versions. Older versions may handle it differently.
### **Post-Scan**
- Review status codes and content length for anomalies.
- Investigate valid responses to confirm accessible or misconfigured VHosts.
