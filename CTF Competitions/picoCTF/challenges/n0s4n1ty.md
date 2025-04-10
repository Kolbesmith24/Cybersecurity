# CTF Write-Up: File Upload to RCE and Privilege Escalation

## Challenge Overview

This challenge involved identifying and exploiting a file upload vulnerability to achieve remote code execution (RCE), followed by privilege escalation to gain root access.

---

## Enumeration and Exploitation

### 1. Discovered Vulnerable File Upload

Identified a file upload feature that allowed files to be uploaded without proper validation or filtering.

### 2. Crafted Shell Payload

Used a reverse shell payload based on the [File Upload Vulnerabilities Cheat Sheet](../../../../Cybersecurity/Hacking/Network%20Security/Vulnerabilities%20and%20Exploits/File%20Upload%20Vulnerabilities/Cheat%20Sheet.md) to gain command execution capability.

### 3. Triggered Remote Code Execution

After the file was uploaded, the application returned the upload path. The file was accessed through the browser, and commands were executed by appending a query parameter:

http://<target>/uploads/shell.php?cmd=<command>


---

## Post-Exploitation and Privilege Escalation

### 4. Verified Limited Access

Initial shell access was limited; attempts to access `/root` were denied, indicating a non-root user.

### 5. Enumerated Sudo Permissions

Ran the following command to identify sudo privileges:

```bash
sudo -l

6. Executed Privileged Binary

Identified a file in /root that could be executed with elevated privileges. Ran the binary using:

sudo /root/<filename>

This successfully escalated privileges and granted root access.
Summary

    File upload vulnerabilities can often be leveraged for RCE when input validation is insufficient.

    Enumerating system permissions (sudo -l) is a critical post-exploitation step.

    Maintaining organized references and cheat sheets streamlines exploit development during CTFs.

Result: Successfully gained root access.
