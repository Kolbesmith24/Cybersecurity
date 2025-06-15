# Server-Side Request Forgery (SSRF)
This type of vulnerability occurs when a web application fetches additional resources from a remote location **based on user-supplied data**, such as a URL.

If the web application relies on a user-supplied URL scheme or protocol, an attacker might be able to cause even further undesired behavior by manipulating the URL scheme. 

For instance, the following URL schemes are commonly used in the exploitation of SSRF vulnerabilities:

- `http://` and `https://`: These URL schemes fetch content via HTTP/S requests. An attacker might use this in the exploitation of SSRF vulnerabilities to bypass WAFs, access restricted endpoints, or access endpoints in the internal network
- `file://`: This URL scheme reads a file from the local file system. An attacker might use this in the exploitation of SSRF vulnerabilities to read local files on the web server (LFI)
- `gopher://`: This protocol can send arbitrary bytes to the specified address. An attacker might use this in the exploitation of SSRF vulnerabilities to send HTTP POST requests with arbitrary payloads or communicate with other services such as SMTP servers or databases
## Confirming SSRF
We can observe sites for areas where we can probe for information from the back-end.
- Can be a button to retrieve:
	- Stock for items that are for sale
	- Checking availability for an appointment
	- etc...

We can capture these requests, and see if there is any parameters that indicate the web server fetches the availability information from a separate system.

We can then change the data to see if we can get the system to make requests on our behalf, such as requesting to connect to a remote shell
## Enumerating the System
We can use the SSRF vulnerability to conduct a port scan of the system to enumerate running services. To achieve this, we need to be able to infer whether a port is open or not from the response to our SSRF payload. 

If we supply a port that we assume is closed (such as `81`) to send against itself with `127.0.0.1`, the response contains an error message:
![image](https://academy.hackthebox.com/storage/modules/145/ssrf/ssrf_identify_5.png)

This enables us to conduct an internal port scan of the web server through the SSRF vulnerability. We can do this using a fuzzer like `ffuf`.

Let us first create a wordlist of the ports we want to scan. In this case, we'll use the first 10,000 ports:
```bash
seq 1 10000 > ports.txt
```

Afterward, we can fuzz all open ports by filtering out responses containing the error message we have identified earlier.
```bash
ffuf -w ./ports.txt -u http://TARGET_IP/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Failed to connect to"
```
## Exploiting SSRF
After discussing how to identify SSRF vulnerabilities and utilize them to enumerate the web server, let us explore further exploitation techniques to increase the impact of SSRF vulnerabilities.
### Accessing Restricted Endpoints
As we have seen, the web application fetches availability information from the URL `dateserver.htb`. However, when we add this domain to our hosts file and attempt to access it, we are unable to do so.

However, we can access and enumerate the domain through the SSRF vulnerability. For instance, we can conduct a directory brute-force attack to enumerate additional endpoints using `ffuf`. To do so, let us first determine the web server's response when we access a non-existing page:
![image](https://academy.hackthebox.com/storage/modules/145/ssrf/ssrf_exploit_2.png)

As we can see, the web server responds with the default Apache 404 response. To also filter out any HTTP 403 responses, we will filter our results based on the string `Server at dateserver.htb Port 80`, which is contained in default Apache error pages. Since the web application runs PHP, we will specify the `.php` extension:
```bash
ffuf -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://dateserver.htb/FUZZ.php&date=2024-01-01" -fr "Server at dateserver.htb Port 80"
```

Once an internal endpoint is identified, we can visit it to gain access.
### Local File Inclusion (LFI)
If we can manipulate the URL scheme to provoke further unexpected behavior, then since the URL scheme is part of the URL supplied to the web application, let us attempt to read local files from the file system using the `file://` URL scheme. We can achieve this by supplying the URL `file:///etc/passwd`
- We can use this to read arbitrary files on the filesystem, including the web application's source code.
### The gopher Protocol
We can use SSRF to access restricted internal endpoints. However, we are restricted to GET requests as there is no way to send a POST request with the `http://` URL scheme. 

For instance, let us consider a different version of the previous web application. Assuming we identified the internal endpoint `/admin.php` just like before, however, this time the response asks us to login:
![image](https://academy.hackthebox.com/storage/modules/145/ssrf/ssrf_exploit_4.png)
- From the HTML form, we can deduce that we need to send a POST request to `/admin.php` containing the password in the `adminpw` POST parameter. However, there is no way to send this POST request using the `http://` URL scheme.

Instead, we can use the [gopher](https://datatracker.ietf.org/doc/html/rfc1436) URL scheme to send arbitrary bytes to a TCP socket. This protocol enables us to create a POST request by building the HTTP request ourselves.

Assuming we want to try common weak passwords, such as `admin`, we can send the following POST request:
```http
POST /admin.php HTTP/1.1
Host: dateserver.htb
Content-Length: 13
Content-Type: application/x-www-form-urlencoded

adminpw=admin
```

We need to URL-encode all special characters to construct a valid gopher URL from this. In particular, spaces (`%20`) and newlines (`%0D%0A`) must be URL-encoded. 

Afterward, we need to prefix the data with the gopher URL scheme, the target host and port, and an underscore, resulting in the following gopher URL:

```url
gopher://dateserver.htb:80/_POST%20/admin.php%20HTTP%2F1.1%0D%0AHost:%20dateserver.htb%0D%0AContent-Length:%2013%0D%0AContent-Type:%20application/x-www-form-urlencoded%0D%0A%0D%0Aadminpw%3Dadmin
```
- Our specified bytes are sent to the target when the web application processes this URL.

Since we carefully chose the bytes to represent a valid POST request, the internal web server accepts our POST request and responds accordingly. However, since we are sending our URL within the HTTP POST parameter `dateserver`, which itself is URL-encoded, we need to URL-encode the entire URL again to ensure the correct format of the URL after the web server accepts it. Otherwise, we will get a `Malformed URL` error. 

After URL encoding the entire gopher URL one more time, we can finally send the following request:
```http
POST /index.php HTTP/1.1
Host: 172.17.0.2
Content-Length: 265
Content-Type: application/x-www-form-urlencoded

dateserver=gopher%3a//dateserver.htb%3a80/_POST%2520/admin.php%2520HTTP%252F1.1%250D%250AHost%3a%2520dateserver.htb%250D%250AContent-Length%3a%252013%250D%250AContent-Type%3a%2520application/x-www-form-urlencoded%250D%250A%250D%250Aadminpw%253Dadmin&date=2024-01-01
```
 - We can use the `gopher` protocol to interact with many internal services, not just HTTP servers. 

#### If we found other services that we want to probe
Imagine a scenario where we identify, through an SSRF vulnerability, that TCP port 25 is open locally. This is the standard port for SMTP servers. 

We can use Gopher to interact with this internal SMTP server as well. 

However, constructing syntactically and semantically correct gopher URLs can take time and effort. Thus, we will utilize the tool [Gopherus](https://github.com/tarunkant/Gopherus) to generate gopher URLs for us. The following services are supported:
- MySQL
- PostgreSQL
- FastCGI
- Redis
- SMTP
- Zabbix
- pymemcache
- rbmemcache
- phpmemcache
- dmpmemcache

To run the tool, we need a valid Python2 installation. Afterward, we can run the tool by executing the Python script downloaded from the GitHub repository:
```shell
python gopherus.py
```

Let us generate a valid SMTP URL by supplying the corresponding argument. The tool asks us to input details about the email we intend to send. Afterward, we are given a valid gopher URL that we can use in our SSRF exploitation:
```shell-session
python2.7 gopherus.py --exploit smtp
```
# Blind SSRF
In many real-world SSRF vulnerabilities, the response is not directly displayed to us. These instances are called `blind` SSRF vulnerabilities because we cannot see the response.
## Identifying Blind SSRF
We can confirm the SSRF vulnerability by supplying a URL to a system under our control and setting up a `netcat` listener with `nc`.

If we attempt to point the web application to itself, we can observe that the response does not contain the HTML response of the coerced request; instead, it simply gives us a basic response. Therefore, this is a blind SSRF vulnerability.
## Exploiting Blind SSRF
we might still be able to conduct a (restricted) local port scan of the system, provided the response differs for open and closed ports. 

In this case, the web application responds with `Something went wrong!` for closed ports.

However, if a port is open and responds with a valid HTTP response, we get a different error message.

While we cannot read local files like before, we can use the same technique to identify existing files on the filesystem. That is because the error message is different for existing and non-existing files, just like it differs for open and closed ports:
![image](https://academy.hackthebox.com/storage/modules/145/ssrf/ssrf_blind_5.png)

For invalid files, the error message is different:

![image](https://academy.hackthebox.com/storage/modules/145/ssrf/ssrf_blind_6.png)

To scan for ports, we can use these errors to identify ports with ffuz:
```bash
ffuf -w ./ports.txt:FUZZ -u http://TARGET_IP/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Something went wrong!"
```
- dont forget to swap out this information for the information in your request that you captured.