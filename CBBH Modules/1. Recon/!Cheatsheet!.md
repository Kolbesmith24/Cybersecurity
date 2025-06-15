# Normal Process of Information Gathering
1. [[WHOIS]]
2. DNS & Subdomains
	1. [[DNS Querying]]
	2. [[DNSEnum]]
	3. [[DNS Zone Transfer]]
	4. [[Virtual Hosts]]
3. [[Fingerprinting]]
4. [[Crawling]]
	1. [[Creepy Crawlies]]
5. [[Search Engine Discovery]]
6. [[Web Archives]]
7. [[Automating Recon]]

# Exam Process
## Create Obsidian Canvas
Create a canvas in obsidian for:
- Each site (branching off to its different endpoints)
- User information notes (store found information of other users such as usernames, emails, login credentials, etc.. Make sure to note which site it came from)

## Scan Ports and Services:
```shell
nmap -v -sV -sC -T4 -p- MACHINE_IP
```

## Scan for Subdomains:
```shell
ffuf -w subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.trilocor.local
```
- If another valid subdomain is found, scan for directories (Step 3) for all subdomains that are found!

## Scan for VHosts
```shell
ffuf -w subdomains-top1million-20000.txt:FUZZ -u http://www.trilocor.local:8080/ -H 'Host: FUZZ.trilocor.local' -fs 0
```
- If another valid subdomain is found, scan for directories (Step 3) for all subdomains that are found!

## Scan for Directories:
```shell
ffuf -w directory-list-2.3-small.txt:FUZZ -u http://www.trilocor.local:8080/FUZZ -recursion -recursion-depth 3 -v
```

If `/api` is found, scan for API endpoints
```shell
ffuf -w common-api-endpoints-mazen160.txt:FUZZ -u 'http://www.trilocor.local:9000/api/FUZZ' -recursion -recursion-depth 5 -fs 0
```

If `DIRECTORY.php` files are found, scan for `.php` directories as well:
```shell
ffuf -w directory-list-2.3-small.txt:FUZZ -u http://www.trilocor.local:8080/FUZZ.php -recursion -recursion-depth 3 -v
```

## Scan for WordPress:
Scan for WordPress and all plugins:
```shell
wpscan --url http://blog.inlanefreight.local -e ap --no-banner --plugins-detection aggressive --plugins-version-detection aggressive --max-threads 60
```

Enumerate all users:
```shell
wpscan --url http://IP:PORT -e u
```

## Scan for WSDL:
First check if it is listed under the `/wsdl` directory:
```html
http://<TARGET IP>:3002/wsdl 
```

If not, we can fuzz for it as follows:
```shell
ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/wsdl?FUZZ'
```
```shell
ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/FUZZ.wsdl'
```
```shell
ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/wsdl?FUZZ.wsdl'
```
```shell
ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/FUZZ.disco'
```
```shell
ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/wsdl?FUZZ.disco'
```

## Scan for API functions:
```shell
ffuf -w burp-parameter-names.txt -u 'http://www.trilocor.local:8088/?FUZZ=test_value' -fs 2457
```

### Scan for API endpoints:
```shell
ffuf -w "/SecLists/Discovery/Web-Content/common-api-endpoints-mazen160.txt" -u 'http://<TARGET IP>:3000/api/FUZZ'
```



## Check site with Wappalyzer Add-On to find what it is running


## Use ZAProxy to scan the site and find all directories / crawling




# After Initial Scans
## Visiting Each Endpoint
1. Go to each and every directory in every subdomain and every VHost that you found, and visit each and every page and make another Note in Canvas for each endpoint
2. When visiting each page, have Developer Tools open on `Network` section, refresh the page, and analyze what requests are being made on that page.
	- Do this with every page you can possible find and note anything suspicious
	- Take note of all directories each request is making these requests to, and note them down if you haven't already
3. Analyze the source-code of each and every page and skim through it
	- If you find any JavaScript, de obfuscate it (https://deobfuscate.io/) and check through it
4. Take note of any cookies 
5. Take note of different possible exploitation opportunities
	- User Input: 
		- How is the user input sent [e.g. JSON, POST Request, XML. etc.]?
		- Is the supplied user input reflected after submission?
		- Is the user input a value to retrieve other information (such as product id)?
		- What is the output after submitting user input? What page does it take you to?
6. Take note of how the URL is displaying the page
	- Is it retrieving files with `?language=`?
	- What parameters is the url stating `?uid=1`?



# REMEMBER
Document EVERYTHING. Take extensive notes of every command and output you get, and every directory you find, as well as what tests you perform on each directory. 

Use Canvas option to organize each endpoint and all tests performed on each one.