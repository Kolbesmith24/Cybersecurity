# Sau Writeup – Hack The Box
## Reconnaissance

To begin, we perform an Nmap scan to enumerate open ports and services on the target machine:

![image](https://github.com/user-attachments/assets/d7f86936-09bd-41f7-a896-7be7e9b334cc)

From the results, we identify the highest open TCP port as 55555. The scan indicates this port is running an HTTP service.

We navigate to http://:55555 in the browser and are presented with a web interface powered by Request-Baskets. The version number is visible in the footer or page source.

![image](https://github.com/user-attachments/assets/6fbf5d1a-d54b-4442-8090-e30817b37cde)

This gives us the answers to the initial enumeration questions. We then search for known vulnerabilities for Request-Baskets version 1.2.1 from 2023 and find a Server-Side Request Forgery (SSRF) vulnerability (CVE-2023-27163).

![image](https://github.com/user-attachments/assets/15aa81c8-a87f-4484-99f3-a61e9873fb8f)

## Exploitation

We begin by exploiting the SSRF vulnerability. Using the web interface, we create a new basket:

![image](https://github.com/user-attachments/assets/1a648304-2d48-4c07-abcd-863a10b54835)

Clicking the gear icon in the top-right, we configure the forward URL to our attack machine’s IP address and port.

![image](https://github.com/user-attachments/assets/f2b0985d-f8a3-441e-abd1-25fa5a3ac19a)

We start a netcat listener on port 80, then trigger the vulnerability by making a curl request to the basket’s endpoint, which forwards to our listener.

![image](https://github.com/user-attachments/assets/75ed2955-73b0-4238-8fe6-fef6a93c8b33)

![image](https://github.com/user-attachments/assets/1eec8a67-6b21-435c-89f3-67402fa47329)

This confirms that the instance is vulnerable to SSRF and port 80 is internally accessible, despite appearing filtered externally via Nmap.

To further explore internal services, we modify the proxy settings:

- Forward URL: `http://127.0.0.1:80`
    
- Enable:
    - **Proxy Response:** This allows the basket to behave as a full proxy: responses from the underlying service configured in forward_url are passed back to clients of original requests. The configuration of basket responses is ignored in this case.
    - **Expand Forward Path:** With this, the forward URL path will be expanded when original HTTP request contains a compound path

We then re-access the basket in our browser:

![image](https://github.com/user-attachments/assets/f98a7486-799f-462f-8fe7-e567ee1640a9)

We discover a Maltrail instance running on the local interface—this is the answer to Task 5.

Searching for vulnerabilities in Maltrail v0.53, we find a command injection PoC involving the `username` parameter with a quick Google search:

![image](https://github.com/user-attachments/assets/740db7e9-bf6d-466b-b4de-46fcd43406b2)

When the attacker provides a username containing a semicolon followed by a command, the shell executes it. We download the provided exploit with wget.

![image](https://github.com/user-attachments/assets/e20adc67-5589-47fc-804d-d36bdec90c0c)

## Getting a Shell

With our exploit ready and a netcat listener running, we trigger the payload to gain a reverse shell.

![image](https://github.com/user-attachments/assets/682129ed-45fa-48ae-8b41-2fd7eab53e62)

![image](https://github.com/user-attachments/assets/f89cb6e4-359e-42c5-bd61-abfdc701cdb4)


Success! We have obtained a shell on the machine.

![image](https://github.com/user-attachments/assets/3c6ae835-294b-41f1-8878-88e4c726a3bb)

Running `whoami` shows us the system user for another task completed:

![image](https://github.com/user-attachments/assets/40b96b0e-ac17-49f4-995a-3d5d08ee538f)

Navigating to the user's home directory, we capture the user flag:

![image](https://github.com/user-attachments/assets/c08b9e74-49a2-470b-b221-dc09cdbac5d6)


## Privilege Escalation

Running `sudo -l` as the `puma` user reveals allowed commands that can be executed as root:

![image](https://github.com/user-attachments/assets/1c9a5d0e-f7f8-4cf2-99fa-8892a2f32447)

This gives us our answer for the next task, and shows us a way to escalate privileges, which we will utilize later on. 

Next, we get the systemd version by running `systemctl --version`:

![image](https://github.com/user-attachments/assets/32251c6f-6417-4ae3-9fac-7c2b328a50fb)

We can then lookup the system version to find a known vulnerability, CVE-2023-26604, which allows for privilege escalation. 

The exploit involves invoking a command using `systemctl` as sudo, and at the prompt to hit 'RETURN', we enter `!/bin/bash` to spawn a root shell.

![image](https://github.com/user-attachments/assets/94a23317-e568-48de-b41a-a4dc6d26ebb6)

Success! We have escalated our privileges.

With root access acquired, we navigate to `/root` and retrieve the root flag in the root.txt file:

![image](https://github.com/user-attachments/assets/01756a9e-19d7-4e65-81cf-12b51c4649ca)

