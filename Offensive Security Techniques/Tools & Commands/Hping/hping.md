
# Quick Guide to `hping` in Linux

`hping` is a network tool used to send custom TCP/IP packets. It’s useful for tasks like scanning ports, testing firewalls, and performing security audits.

---

## ⚙Basic Syntax

```bash
hping3 [options] [target]
```

`hping3` is the most common version. You can add options to control the type of packet sent (TCP, UDP, ICMP), the destination port, and more.

---

## Common Use Cases

### 1. Ping with TCP (instead of ICMP)
```bash
hping3 -S -p 80 example.com
```
- `-S`: Sends a TCP SYN packet
- `-p 80`: Sends it to port 80 (web server)

Use this when ICMP ping is blocked.

---

### 2. Perform a SYN Port Scan
```bash
hping3 -S -p ++50 example.com
```
- `-p ++50`: Scan ports starting from 50 and up
- SYN scan can reveal open ports without completing the handshake.

---

### 3. Send a UDP Packet
```bash
hping3 --udp -p 53 example.com
```
- `--udp`: Sends a UDP packet
- `-p 53`: Targets DNS port (for example)

---

### 4. Traceroute with TCP Packets
```bash
hping3 -S -p 80 --traceroute example.com
```
- Shows the path packets take to the host like `traceroute`, but using TCP.

---

### 5. Send ICMP Echo Request (like `ping`)
```bash
hping3 --icmp example.com
```
- Mimics regular ping behavior with more control.

---

### 6. Flood Target with Packets (DoS Testing)
```bash
hping3 -S --flood -V -p 80 example.com
```
- `--flood`: Sends packets as fast as possible
- `-V`: Verbose mode
- Great for stress-testing firewalls in labs

---

## Handy Options Summary

| Option        | Description                          |
|---------------|--------------------------------------|
| `-S`          | Send SYN packet                      |
| `-A`          | Send ACK packet                      |
| `--icmp`      | Send ICMP echo request               |
| `--udp`       | Send UDP packet                      |
| `-p [port]`   | Set destination port                 |
| `-p ++[port]` | Increment port number after each send|
| `--flood`     | Send packets rapidly (no reply wait) |
| `--traceroute`| Perform TCP-based traceroute         |
| `-V`          | Verbose output                       |

---

## 🧪 Good to Know

- `hping` can spoof IPs and craft custom headers.
- Useful for testing firewalls, IDS/IPS systems, and understanding packet behavior.

---

## 📘 Learn More

```bash
man hping3
hping3 --help
```

---
