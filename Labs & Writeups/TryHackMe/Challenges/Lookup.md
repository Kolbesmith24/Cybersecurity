# TryHackMe - Lookup Writeup

### Reconnaissance

Upon accessing the target machine, we noted that two services were running:

- **SSH** on port **22**
    
- **HTTP** on port **80**
    

We began our investigation by navigating to the HTTP service hosted on port 80. The web application presented a login prompt. Initial attempts to perform SQL injection yielded no results, indicating the login form was not vulnerable to basic SQLi techniques.

### Enumeration

Further testing of the login form revealed interesting behavior: the server responded with a generic "incorrect password" message, but if the username was valid, the message changed to reflect that only the password was incorrect. This difference in error responses allowed for **username enumeration**.

To automate this, we used a script (assisted by ChatGPT) to iterate through a list of potential usernames (e.g., from DirBuster’s default wordlist). This process identified **`jose`** as a valid user.

After successfully logging in with credentials **`jose:password123`**, we were redirected to a web-based file manager, **elFinder**.

### Exploitation

Within elFinder, we discovered a file containing the credentials **`think:nopassword`**, suggesting another potential user.

We investigated the elFinder version in use and discovered a publicly available **Remote Code Execution (RCE) proof-of-concept (PoC)** exploit for this version. Using this exploit, we were able to gain a **remote shell** on the target system.

While inspecting the system, we came across a binary at **`/usr/sbin/pwm`**. This binary was unusual, as it's not commonly found on a default Linux installation. We examined the file's permissions:

```bash
ls -la /usr/sbin/pwm
```

Running the binary simply executed the `id` command, printing out the current user's UID, GID, and group memberships. However, crucially, the binary **did not use an absolute path** to `id`—instead, it relied on the system’s `$PATH` environment variable to locate the executable.

This presented a classic **PATH hijacking** opportunity.

We added a custom directory (**`/tmp`**) to the front of our PATH and created a fake `id` script that would return spoofed credentials, mimicking the output for user `think`:

```bash
echo 'uid=33(think) gid=33(think) groups=33(think)' > /tmp/id
chmod +x /tmp/id
export PATH=/tmp:$PATH
/usr/sbin/pwm
```

Upon rerunning the binary, it printed a series of strings that resembled passwords, possibly intended for the `think` user.

We compiled these strings into a wordlist and used **Hydra** to perform a brute-force SSH attack against the `think` account:

```bash
hydra -l think -P passwords.txt ssh://10.10.32.68
```

Hydra successfully found valid SSH credentials, and we logged in as `think`, retrieving the **user flag** located in the user's home directory.

### Privilege Escalation

Running `sudo -l` revealed that the `think` user had permission to run the `look` binary with `sudo`:

```bash
sudo -l
# Output:
# (think) NOPASSWD: /usr/bin/look
```

We consulted **GTFOBins** and found that `look` can be abused to gain shell access if it is run with elevated privileges. Using the documented method, we escalated privileges to root.

Finally, we accessed the root user's home directory and retrieved the **root flag** from `root.txt`.