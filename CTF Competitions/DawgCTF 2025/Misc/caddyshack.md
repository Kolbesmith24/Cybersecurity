# Caddyshack - DawgCTF 2025
The challenge instructs us to locate and connect to a server hosted at `caddyshack.umbccd.net`.
## Initial Reconnaissance
Attempting to access the website via a web browser resulted in a connection error — the server appeared unreachable over HTTP/HTTPS.

![image](https://github.com/user-attachments/assets/93c316fe-0150-4412-b343-ebeeb747075c)

To identify open ports and services, I performed an `nmap` scan:
```
nmap -v -sV -sC -T4 caddyshack.umbccd.net
```
![image](https://github.com/user-attachments/assets/15979742-c4d1-4f99-b1ef-e86b24d8d3d1)
![image](https://github.com/user-attachments/assets/fb921c3f-e235-487e-8489-2f83f8a7bb44)

The scan revealed an open port:
- **Port 70**: Running the **Gopher** protocol

Additionally, the scan output included a reference to a file named `Flag.txt`.

---
## Exploring the Gopher Service
Knowing that Gopher is a protocol designed for distributing, searching, and retrieving documents over the Internet, I accessed the server using the Gopher protocol by navigating to:
```
gopher://caddyshack.umbccd.net:70
```
![image](https://github.com/user-attachments/assets/1a6fb7f8-9bb4-4bcb-a0eb-06af61fef2b8)

This presented a Gopher-style directory listing. Among the entries, there was a file named `File.txt`.

Appending the file to the Gopher path:

```
gopher://caddyshack.umbccd.net:70/File.txt
```

Navigating to this link revealed the challenge **flag**.
![image](https://github.com/user-attachments/assets/902b2c05-9136-4a64-a9fa-8fa93fd15269)

