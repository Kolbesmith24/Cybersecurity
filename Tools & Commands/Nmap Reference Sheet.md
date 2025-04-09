# Reference
*nmap* syntax:
```
nmap OPTIONS MACHINE_IP
```

Best syntax for CTFs:
```
nmap -v -sV -sC -T4 MACHINE_IP
```
## Options
*-sN*: Least chances of being caught, but cant guarantee the ports are open
*-sS*: TCP SYN scan. Decreases chance of being caught
*-p-*: Scans all ports
*-v*: Verbose output
*-sV*: Determines service and version of open ports (not possible with *-sS*)
*-sC*: Uses common scripts 
*-oA*: Generates all versions for output
*-O*: Detects(guesses) OS and the version

Can scan a range of IP addresses: `10.11.12.15-20`
Can provide a file as input for list of targets: `nmap -iL list_of_hosts.txt`
#### Control timing: T0-5
- *0* = extremely slow
- *1* = sneaky
- *2* = polite
- *3* = normal
- *4* = aggressive
- *5* = insane

*3* is the default
*4* is generally used for CTFs, where *1* is used in real engagements
### Results:
When querying systems with *nmap*, there are multiple results:
1. **Open**: indicates that a service is listening on the specified port.
2. **Closed**: indicates that no service is listening on the specified port, although the port is accessible. By accessible, we mean that it is reachable and is not blocked by a firewall or other security appliances/programs.
3. **Filtered**: means that Nmap cannot determine if the port is open or closed because the port is not accessible. This state is usually due to a firewall preventing Nmap from reaching that port. Nmap’s packets may be blocked from reaching the port; alternatively, the responses are blocked from reaching Nmap’s host.
4. **Unfiltered**: means that Nmap cannot determine if the port is open or closed, although the port is accessible. This state is encountered when using an ACK scan `-sA`.
5. **Open|Filtered**: This means that Nmap cannot determine whether the port is open or filtered.
6. **Closed|Filtered**: This means that Nmap cannot decide whether a port is closed or filtered.

# Network Protocols & Port Reference Sheet

| Protocol | TCP Port | Encryption | Application/Service         |
|----------|----------|------------|------------------------------|
| FTP      | 21       | Cleartext  | File Transfer                |
| FTPS     | 990      | Encrypted  | File Transfer (Secure)       |
| SFTP     | 22       | Encrypted  | File Transfer over SSH       |
| HTTP     | 80       | Cleartext  | Web Browsing                 |
| HTTPS    | 443      | Encrypted  | Secure Web Browsing          |
| IMAP     | 143      | Cleartext  | Email Retrieval              |
| IMAPS    | 993      | Encrypted  | Secure Email Retrieval       |
| POP3     | 110      | Cleartext  | Email Retrieval              |
| POP3S    | 995      | Encrypted  | Secure Email Retrieval       |
| SMTP     | 25       | Cleartext  | Email Sending (Unsecured)    |
| SMTPS    | 465      | Encrypted  | Secure Email Sending         |
| SSH      | 22       | Encrypted  | Secure Shell Access          |
| Telnet   | 23       | Cleartext  | Remote Shell Access          |

## Notes

- **SFTP vs FTPS**: SFTP uses SSH for secure file transfer, while FTPS uses SSL/TLS.
- **SMTP Port 25** is often blocked by ISPs due to spam and is mainly used for mail relay between servers.
- Use **IMAPS** or **POP3S** instead of their cleartext counterparts for secure email access.
- **Telnet** is deprecated in secure environments—use **SSH** instead.


