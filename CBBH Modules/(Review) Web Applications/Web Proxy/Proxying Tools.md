# Different Tools
## Proxychains
[[Proxychains]]
Routes all traffic coming from any command-line tool to any proxy we specify.
## Nmap
We can use the `--proxies` flag. We should also add the `-Pn` flag to skip host discovery (as recommended on the man page). Finally, we'll also use the `-sC` flag to examine what an nmap script scan does.

Example:
```
nmap --proxies http://127.0.0.1:8080 SERVER_IP -pPORT -Pn -sC
```

If we go to our web proxy tool, we will see all of the requests made by nmap in the proxy history.
## Metasploit
We should begin by starting Metasploit with `msfconsole`. Then, to set a proxy for any exploit within Metasploit, we can use the `set PROXIES` flag. 

Let's try the `robots_txt` scanner as an example and run it against a previous exercise:
```
msfconsole

use auxiliary/scanner/http/robots_txt

set PROXIES HTTP:127.0.0.1:8080

set RHOST SERVER_IP

set RPORT PORT

run
```

Once again, we can go back to our web proxy tool of choice (burp) and examine the proxy history to view all sent requests.