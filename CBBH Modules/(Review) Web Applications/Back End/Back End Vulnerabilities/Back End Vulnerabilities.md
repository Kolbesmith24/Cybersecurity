# Back End Vulnerabilities
The below examples are some of the most common vulnerability types for web applications, part of [OWASP Top 10](https://owasp.org/www-project-top-ten/) vulnerabilities for web applications.
## Broken Authentication/Access Control

`Broken authentication` and `Broken Access Control` are among the most common and most dangerous vulnerabilities for web applications.

`Broken Authentication` refers to vulnerabilities that allow attackers to bypass authentication functions. For example, this may allow an attacker to login without having a valid set of credentials or allow a normal user to become an administrator without having the privileges to do so.

`Broken Access Control` refers to vulnerabilities that allow attackers to access pages and features they should not have access to. For example, a normal user gaining access to the admin panel.

For example, `College Management System 1.2` has a simple [Auth Bypass](https://www.exploit-db.com/exploits/47388) vulnerability that allows us to login without having an account, by inputting the following for the email field: `' or 0=0 #`, and using any password with it.
## Malicious File Upload
Another common way to gain control over web applications is through uploading malicious scripts. If the web application has a file upload feature and does not properly validate the uploaded files, we may upload a malicious script (i.e., a `PHP` script), which will allow us to execute commands on the remote server.
## Command Injection
Many web applications execute local Operating System commands to perform certain processes. For example, a web application may install a plugin of our choosing by executing an OS command that downloads that plugin, using the plugin name provided. 

If not properly filtered and sanitized, attackers may be able to inject another command to be executed alongside the originally intended command (i.e., as the plugin name), which allows them to directly execute commands on the back end server and gain control over it.
## SQL Injection (SQLi)
Similarly to a Command Injection vulnerability, this vulnerability may occur when the web application executes a SQL query, including a value taken from user-supplied input.
# Public Vulnerabilities
## Public CVE
Searching for public exploits is one of the very first steps we must go through for web applications.
`Tip: The first step is to identify the version of the web application.`

Once we identify the web application version, we can search Google for public exploits for this version of the web application. We can also utilize online exploit databases, like [Exploit DB](https://www.exploit-db.com), [Rapid7 DB](https://www.rapid7.com/db/), or [Vulnerability Lab](https://www.vulnerability-lab.com).

We would usually be interested in exploits with a CVE score of 8-10 or exploits that lead to `Remote Code Execution`. Other types of public exploits should also be considered if none of the above is available.
## Common Vulnerability Scoring System (CVSS)
An open-source industry standard for assessing the severity of security vulnerabilities.

The NVD provides a [CVSS v2 calculator](https://nvd.nist.gov/vuln-metrics/cvss/v2-calculator) and a [CVSS v3 calculator](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator) that organizations can use to factor additional risk from `Temporal` and `Environmental` data unique to them.
## Back-end Server Vulnerabilities
Like public vulnerabilities for web applications, we should also consider looking for vulnerabilities for other back end components, like the back end server or the webserver.

The most critical vulnerabilities for back-end components are found in web servers, as they are publicly accessible over the `TCP` protocol. An example of a well-known web server vulnerability is the `Shell-Shock`, which affected Apache web servers released during and before 2014 and utilized `HTTP` requests to gain remote control over the back-end server.

As for vulnerabilities in the back-end server or the database, they are usually utilized after gaining local access to the back-end server or back-end network, which may be gained through `external` vulnerabilities or during internal penetration testing. They are usually used to gain high privileged access on the back-end server or the back-end network or gain control over other servers within the same network.

Although not directly exploitable externally, these vulnerabilities are still critical and need to be patched to protect the entire web application from being compromised.