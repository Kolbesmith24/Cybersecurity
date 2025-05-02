# HTB - Academy Writeup

This write-up walks through the steps taken to compromise the *Academy* machine on Hack The Box, following a structured approach with distinct reconnaissance, discovery, exploitation, and privilege escalation phases.

---

## Reconnaissance

We are initially provided with the target machine's IP address. To make hostname resolution easier, we add it to our `/etc/hosts` file:

![image](https://github.com/user-attachments/assets/315c42e0-3ef0-4f9f-8f4e-8b8598779aa3)


We begin by scanning the host using `nmap` to identify open ports and services, as well as using `gobuster` to discover hidden directories:

![image](https://github.com/user-attachments/assets/951fc3bb-a86d-4f9a-b282-3e2149cdc69a)


One interesting find is an `admin.php` page, which leads to a login form:

![image](https://github.com/user-attachments/assets/f3ed6646-df8d-49e7-a66f-0821225006b4)


---

## Discovery 

We register a new user account on the application. Upon login, we are directed to the HTB Academy dashboard containing various learning modules:

![image](https://github.com/user-attachments/assets/aa0baeeb-7a7e-4125-a5d6-a854911e0673)


While clicking on different navigation options such as “Profile” or “Billing”, the UI does not change, but the URL does, suggesting that client-side routing or hidden server-side logic may be at play:

![image](https://github.com/user-attachments/assets/c4695f1a-e4f3-4ace-8928-211f97a4c990)


Intercepting the registration request in Burp reveals that a `roleid` parameter is set for new users:

![image](https://github.com/user-attachments/assets/aedb049f-596e-416f-ab53-d7bc6869e868)


We modify the `roleid` value to `1` (which we assume might correspond to an admin role) and are successfully redirected to an admin portal after re-registering and logging in:

![image](https://github.com/user-attachments/assets/c1eef3bf-d7b2-42bb-b53e-fb5da793c303)


We also add the internal staging site domain (`dev-staging-01.academy.htb`) to our `/etc/hosts` file to access it:

![image](https://github.com/user-attachments/assets/75169aae-22c7-42a9-9850-d480d15ab15a)



---

## Exploitation

Navigating to the staging environment, we are greeted with a Laravel error screen — indicative of debug mode being enabled. This reveals the sensitive `APP_KEY` value in plaintext:

![image](https://github.com/user-attachments/assets/b52e938f-d083-423e-99ef-9d1956bd8616)


With the application key exposed, we search for relevant Laravel vulnerabilities and find **CVE-2018-15133** — a known vulnerability that enables remote code execution when the app key is known and the XSRF token is manipulated.

![image](https://github.com/user-attachments/assets/c14c6c25-a0eb-4669-b517-751e2ef568de)


We set up a local Python environment, install dependencies, and run an exploit script leveraging PHPGGC to craft a payload:

![image](https://github.com/user-attachments/assets/d5cba7a9-1b7a-46e5-b602-26a821abb273)

![image](https://github.com/user-attachments/assets/dfa434e2-332b-46d8-8fd8-8509218ec673)


We gain a reverse shell but are restricted in movement. After troubleshooting, we decide to spawn a more stable reverse shell using a PHP one-liner:

![image](https://github.com/user-attachments/assets/960b3cac-83a6-4484-8d06-b54fc8c5ce72)

Starting our nc listener:
![image](https://github.com/user-attachments/assets/5d4cbb47-3a37-40d9-a9a2-4206faf62725)

Connect to our listener with PHP:
![image](https://github.com/user-attachments/assets/678d55d3-d86b-49b2-a3f6-d7720a8af2b5)

Successfully connected:
![image](https://github.com/user-attachments/assets/cbd84bee-6761-4f7f-bc13-74c9fb1a118a)

Once inside, I found a user.txt file and attempt to read `user.txt`, but permissions are denied:

![image](https://github.com/user-attachments/assets/fa425cb2-6041-40d0-b2d8-91fa694c1a93)

After further enumeration, we find a file containing credentials for another user:

![image](https://github.com/user-attachments/assets/cf87565a-c881-4b0c-87c8-5a8fe6030b74)


We switch users using `su` and successfully access the `user.txt` flag:

![image](https://github.com/user-attachments/assets/2ce0f431-b88a-4c02-b62f-ba4b214fb19d)

![image](https://github.com/user-attachments/assets/2f0b703d-81a3-4370-ae63-2981bac14a55)

---

## Privilege Escalation 

We begin by searching for SUID/SGID binaries, but nothing useful is found:

![image](https://github.com/user-attachments/assets/ce90ce4c-7c2f-42a2-aefc-0db6844a8fc2)

We check our `$PATH` to determine if we can manipulate it:

![image](https://github.com/user-attachments/assets/c1990ca9-2dab-4089-a227-13e651c5db51)


Attempts to escalate privileges using binaries in writable paths are unsuccessful. However, running `id` reveals the user is part of the `adm` group:

![image](https://github.com/user-attachments/assets/945673d2-486a-49c4-b6b5-c31a4821ed44)


This gives access to log files under `/var/log`. If TTY auditing is enabled, typed commands — including passwords — are recorded in hex format:

![image](https://github.com/user-attachments/assets/b05e9dfc-1d0f-4514-870f-2debf6e7fafb)

We use `aureport` to analyze logs and find another user's password:

![image](https://github.com/user-attachments/assets/b2247c46-486a-4d2a-8588-7e0330d8fd90)


We use the recovered password `mrb3n_Ac@d3my!` to `su` into the user `mrb3n`, then list sudo privileges:

![image](https://github.com/user-attachments/assets/2ef45012-7eb2-4ccc-b498-cde37a01cd61)


The user can run `/usr/bin/composer` as root. According to [GTFOBins](https://gtfobins.github.io/gtfobins/composer/), this can be leveraged for privilege escalation. We use the documented method to obtain a root shell:

![image](https://github.com/user-attachments/assets/6c1c2ae1-a269-497a-9823-fa29bc9772d8)

We then stabalize the shell and run the commands:

![image](https://github.com/user-attachments/assets/24398ce1-9e26-4ceb-848e-a42bb116c1b4)

Finally, we are now root, and we can retrieve the root.txt flag!

---

