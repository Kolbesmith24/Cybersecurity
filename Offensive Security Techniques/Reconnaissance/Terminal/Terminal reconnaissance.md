# Passive / Active Reconnaissance

## Terminal: Passive Reconnaissance

Passive reconnaissance is the process of gathering information about a target without directly interacting with it. This approach minimizes the chances of detection by the target.

### WHOIS

The `whois` command queries databases that store registered users or assignees of an Internet resource, such as a domain name or an IP address. This tool helps gather information about domain ownership, registrar details, contact information, creation and expiration dates, and name servers.

**Command Syntax:**
```bash
whois DOMAIN_NAME
```

**Example:**
```bash
whois tryhackme.com
```

### nslookup

`nslookup` is a command-line tool used for querying the Domain Name System (DNS) to obtain domain name or IP address mapping and DNS record details. It is commonly used to find IP addresses associated with domain names.

**Command Syntax:**
```bash
nslookup -type=OPTIONS DOMAIN_NAME SERVER
```

- **OPTIONS**: Defines the type of DNS record to look up:
  - `A`: IPv4 address
  - `AAAA`: IPv6 address
  - `CNAME`: Canonical name
  - `MX`: Mail exchange servers
  - `TXT`: Text records
- **DOMAIN_NAME**: The domain being queried.
- **SERVER**: Optional DNS server (e.g., Google's 8.8.8.8, Cloudflare's 1.1.1.1)

**Example:**
```bash
nslookup -type=A tryhackme.com
```

### dig

`dig` (Domain Information Groper) is a powerful command-line tool for querying DNS name servers. It provides detailed responses, making it useful for troubleshooting DNS problems and performing advanced DNS queries.

**Command Syntax:**
```bash
dig DOMAIN_NAME TYPE
```

- **TYPE**: The DNS record type (e.g., MX, A, AAAA, etc.)

**Example:**
```bash
dig tryhackme.com MX
```

## Terminal: Active Reconnaissance

Active reconnaissance involves direct interaction with the target system or network to gather information. This method is more likely to be detected but provides more detailed data.

### Traceroute

`traceroute` (or `tracert` on Windows) tracks the path that a packet takes from the source system to a destination system. It shows each hop (router) along the path and the time taken to reach each hop.

**Linux Syntax:**
```bash
traceroute MACHINE_IPorHOSTNAME
```

**Windows Syntax:**
```bash
tracert MACHINE_IPorHOSTNAME
```

### Telnet

`telnet` is a protocol and command-line tool used to connect to remote systems over TCP/IP. It's often used to test connectivity to specific ports and services.

**Command Syntax:**
```bash
telnet MACHINE_IP PORT
```

**Example:**
```bash
telnet 192.168.1.1 80
```

### Ping

`ping` checks the availability of a host on a network by sending ICMP Echo Request packets and waiting for a reply. It's commonly used to determine if a machine is online and measure response time.

**Command Syntax:**
```bash
ping OPTION MACHINE_IP
```

- `-c`: Specifies the number of ping packets to send (Linux)
- `-n`: Specifies the number of ping packets to send (Windows)

**Examples:**
```bash
ping -c 4 192.168.1.1   # Linux
ping -n 4 192.168.1.1   # Windows
```
