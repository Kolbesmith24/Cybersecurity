# Authentication Vulnerabilities
## Login Forms
### Enumerating Users
1. Test to see if you get different error messages when providing a valid username & invalid password, compared to providing both invalid username & invalid password. 
	- If you do get different errors, you can enumerate usernames (can also do this on the register form):
```shell
ffuf -w xato-net-10-million-usernames.txt -u http://www.trilocor.local/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown user"
```
#### Test Default Credentials
1. First, identify the software being used
2. Lookup software to see if it uses default password and what they are 
	- List of default credentials for softwares: (https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials)
3. Test passwords on the usernames found from previous step
#### Brute-Force Password Reset Tokens (else move to next section)
Once a username is found, try to reset their password, and check if it asks to supply a reset token for that user. If so, brute-force the reset token:
```shell
seq -w 0 9999 > tokens.txt
```
```shell
ffuf -w tokens.txt -u http://www.trilocor.local/reset_password.php?token=FUZZ -fr "The provided token is invalid"
```
- If it isn't displayed in the URL, refer to previous `ffuf` command and send as POST data
##### If a security prompt/question is required to reset a password:
```http
POST /security_question.php HTTP/1.1
Host: pwreset.htb
Content-Length: 43
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=39b54j201u3rhu4tab1pvdb4pv

security_response=London&username=htb-stdnt
```

We can try to change the request to directly just skip the prompt and change the password:
```http
POST /reset_password.php HTTP/1.1
Host: pwreset.htb
Content-Length: 32
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=39b54j201u3rhu4tab1pvdb4pv

password=P@$$w0rd&username=admin
```

#### Brute-Force 2FA Codes
After gaining credential pair, if you need a 2FA code, you can attempt to brute force this as well:
```shell
ffuf -w tokens.txt -u http://www.trilocor.local/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=0sm6tj93knc494ulmtaiglc6n9" -d "otp=FUZZ" -fr "Invalid 2FA Code" -t 60
```
- If this doesn't work, intercept the request for this page in Burp, send to repeater, change the request to a `GET` request and change the destination to the users profile (e.g. `profile.php` or the admin interface `admin.php`)
## Unauthorized Pages
### `302` Redirected / Targeting Restricted Pages
Any time you get a `302` redirect (or redirected in general) while attempting to access a page:
1. Intercept the request in Burp
2. Right-click and select `Do intercept` -> `Response to this request`
3. Forward request and change the status code from `302 Found` to `200 OK` in the response
4. Forward the response and check browser to see if the page displayed
### Not Authorized to Other Pages While Logged In
If you're logged in and trying to gain access to a page (`/admin`) but are unauthorized, try:
1. Remove your `uid` (or other form of identification) from our request to `/admin`
2. If you are sent back to the login screen and have the same `PHPSESSID` cookie, we can assume the parameter `uid` is related to authentication
3. Brute-force the `uid` of the admin to gain access to the page
## Weak / Predictable Session Tokens
If you see a weak / predictable session token / cookie, you can:
1. Brute force it if it is weak / short, or;
2. Decrypt it and analyze its encryption mechanism, then change the info that it is encrypting to admin
## Session tokens aren't unique to each session
If your session cookie value doesn't change AFTER signing in:
1. Obtain a valid session token by signing in (ex: `sid=a1b2c3d4e5f6`)
2. Log out
3. Send the victim a link with your session token (ex: `http://vulnerable.htb/?sid=a1b2c3d4e5f6 )
	- When they click the link, their session cookie is now this value
4. Now attempt to access a resource you would access when you're logged in with the same session token to hijack the victim's session
