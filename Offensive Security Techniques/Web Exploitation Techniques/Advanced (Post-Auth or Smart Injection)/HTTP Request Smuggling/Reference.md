# What is it?
Learn more at: https://portswigger.net/research/http-desync-attacks-request-smuggling-reborn
A technique for interfering with the way a web site processes sequences of HTTP requests that are received from one or more users.
- allows an attacker to bypass security controls, gain unauthorized access to sensitive data, and directly compromise other application users.

Request smuggling is primarily associated with HTTP/1 requests. However, websites that support HTTP/2 may be vulnerable, depending on their back-end architecture.
## What happens in an HTTP request smuggling attack?
Users send requests to a front-end server (sometimes called a load balancer or reverse proxy) and this server forwards requests to one or more back-end servers.

When the front-end server forwards HTTP requests to a back-end server, it typically sends several requests over the same back-end network connection, because this is much more efficient and performant. The protocol is very simple; HTTP requests are sent one after another, and the receiving server has to determine where one request ends and the next one begins:
![Pasted image 20250430214053.png](/mnt/c/Users/kolbe/OneDrive/Obsidian Vault/Cybersecurity/ScreenshotsPasted%20image%2020250430214053.png)

In this situation, it is crucial that the front-end and back-end systems agree about the boundaries between requests. Otherwise, an attacker might be able to send an ambiguous request that gets interpreted differently by the front-end and back-end systems:
![Pasted image 20250430214125.png](/mnt/c/Users/kolbe/OneDrive/Obsidian Vault/Cybersecurity/ScreenshotsPasted%20image%2020250430214125.png)
- Here, the attacker causes part of their front-end request to be interpreted by the back-end server as the start of the next request. It is effectively prepended to the next request, and so can interfere with the way the application processes that request. This is request smuggling
## How do HTTP request smuggling vulnerabilities arise?
The HTTP/1 specification provides two different ways to specify where a request ends: the `Content-Length` header and the `Transfer-Encoding` header.

The `Content-Length` header: specifies the length of the message body in bytes. For example:

```
POST /search HTTP/1.1 
Host: normal-website.com 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 11 

q=smuggling
```

The `Transfer-Encoding` header can be used to specify that the message body uses chunked encoding. This means that the message body contains one or more chunks of data. Each chunk consists of the chunk size in bytes (expressed in hexadecimal), followed by a newline, followed by the chunk contents. The message is terminated with a chunk of size zero. For example:
```
POST /search HTTP/1.1 
Host: normal-website.com 
Content-Type: application/x-www-form-urlencoded 
Transfer-Encoding: chunked 

b 
q=smuggling 
0
```
*Note:* 
- Burp Suite automatically unpacks chunked encoding to make messages easier to view and edit.
- Browsers do not normally use chunked encoding in requests, and it is normally seen only in server responses.

As the HTTP/1 specification provides two different methods for specifying the length of HTTP messages, it is possible for a single message to use both methods at once, such that they conflict with each other. 

If both the `Content-Length` and `Transfer-Encoding` headers are present, then the `Content-Length` header should be ignored. This might be sufficient to avoid ambiguity when only a single server is in play, but not when two or more servers are chained together. In this situation, problems can arise for two reasons:
1. Some servers do not support the `Transfer-Encoding` header in requests.
2. Some servers that do support the `Transfer-Encoding` header can be induced not to process it if the header is obfuscated in some way.

If the front-end and back-end servers behave differently in relation to the (possibly obfuscated) `Transfer-Encoding` header, then they might disagree about the boundaries between successive requests, leading to request smuggling vulnerabilities.

*Note: Websites that use HTTP/2 end-to-end are inherently immune to request smuggling attacks.*
- *HTTP/2 specification introduces a single, robust mechanism for specifying the length of a request, there is no way for an attacker to introduce the required ambiguity.*
- *However, many websites have an HTTP/2-speaking front-end server, but deploy this in front of back-end infrastructure that only supports HTTP/1. This means that the front-end effectively has to translate the requests it receives into HTTP/1. This process is known as HTTP downgrading.*
- *You can learn more at https://portswigger.net/web-security/request-smuggling/advanced*
- *If a message is received with both a Transfer-Encoding header field and a Content-Length header field, the latter MUST be ignored.*
## Performing HTTP Request Smuggling attacks
Classic request smuggling attacks involve placing both the `Content-Length` header and the `Transfer-Encoding` header into a single HTTP/1 request and manipulating these so that the front-end and back-end servers process the request differently. The exact way in which this is done depends on the behavior of the two servers:
- CL.TE: the front-end server uses the `Content-Length` header and the back-end server uses the `Transfer-Encoding` header.
- TE.CL: the front-end server uses the `Transfer-Encoding` header and the back-end server uses the `Content-Length` header.
- TE.TE: the front-end and back-end servers both support the `Transfer-Encoding` header, but one of the servers can be induced not to process it by obfuscating the header in some way.

*Note: These techniques are only possible using HTTP/1 requests. Browsers and other clients, including Burp, use HTTP/2 by default to communicate with servers that explicitly advertise support for it during the TLS handshake.*
- *As a result, when testing sites with HTTP/2 support, you need to manually switch protocols in Burp Repeater. You can do this from the **Request attributes** section of the **Inspector** panel.*
### CL.TE
Here, the front-end server uses the `Content-Length` header and the back-end server uses the `Transfer-Encoding` header. We can perform the following:
```
POST /about HTTP/1.1   
Host: example.com   
Transfer-Encoding: chunked   
Content-Length: 4      

1   
Z   
Q
```

Thanks to the short Content-Length, the front end will forward the `1` & `Z` text only, and the back end will time out while waiting for the next chunk size. This will cause an observable time delay.
- **If TE.TE or CL.CL**: Request will either be rejected by the front-end or harmlessly processed by both systems.
- **If TE.CL:** Front-end will reject the message without ever forwarding it to the back-end, thanks to the invalid chunk size 'Q'. This prevents the back-end socket from being poisoned.
![Pasted image 20250501090815.png](/mnt/c/Users/kolbe/OneDrive/Obsidian Vault/Cybersecurity/ScreenshotsPasted%20image%2020250501090815.png)
![Pasted image 20250501091009.png](/mnt/c/Users/kolbe/OneDrive/Obsidian Vault/Cybersecurity/ScreenshotsPasted%20image%2020250501091009.png)
### TE.CL
Here, the front-end server uses the `Transfer-Encoding` header and the back-end server uses the `Content-Length` header. We can perform the following:
```
POST /about HTTP/1.1   
Host: example.com   
Transfer-Encoding: chunked   
Content-Length: 6      

0      

X
```
Since the `0` terminates the chunk, the front-end will forward the `X` to the back end. The back-end will then time out waiting for the `X` to arrive.

**If CL.TE:** This approach will poison the back-end socket with an X

***NOTE: if CL.TE, this may potentially harm legitimate users. Fortunately, by always running the prior detection method first, we can rule out that possibility.***
### TE.TE
Here, the front-end and back-end servers both support the `Transfer-Encoding` header, but one of the servers can be induced not to process it by obfuscating the header in some way.

There are potentially endless ways to obfuscate the `Transfer-Encoding` header. For example:
```
Transfer-Encoding: xchunked

Transfer-Encoding : chunked

Transfer-Encoding: chunked
Transfer-Encoding: x

Transfer-Encoding:[tab]chunked

[space] Transfer-Encoding: chunked

X: X[\n]Transfer-Encoding: chunked

Transfer-Encoding
: chunked
```
Each of these techniques involves a subtle departure from the HTTP specification. Real-world code that implements a protocol specification rarely adheres to it with absolute precision, and it is common for different implementations to tolerate different variations from the specification.

To uncover a TE.TE vulnerability, it is necessary to find some variation of the `Transfer-Encoding` header such that only one of the front-end or back-end servers processes it, while the other server ignores it.

Depending on whether it is the front-end or the back-end server that can be induced not to process the obfuscated `Transfer-Encoding` header, the remainder of the attack will take the same form as for the CL.TE or TE.CL vulnerabilities already described.
### Confirming The Attack
The next step towards demonstrating the full potential of request smuggling is to prove back-end socket poisoning is possible. To do this we'll issue a request designed to poison a back-end socket, followed by a request which will hopefully fall victim to the poison, visibly altering the response.
- If the first request causes an error the back-end server may decide to close the connection, discarding the poisoned buffer and breaking the attack. Try to avoid this by targeting an endpoint that is designed to accept a POST request, and preserving any expected GET/POST parameters.

Some sites have multiple distinct back-end systems, with the front-end looking at each request's method, URL, and headers to decide where to route it.
- If the victim request gets routed to a different back-end from the attack request, the attack will fail. As such, the 'attack' and 'victim' requests should initially be as similar as possible.\
#### CL.TE Socket Poisoning
If a request looks like:
```
POST / search HTTP/1.1
Host: example. com
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

q=smuggling
```

An attempt at CL.TE socket poisoning would look like:
```
POST / search HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 53
Transfer-Encoding: zchunked

17
=x&q=smuggling&x=
0

GET /404 HTTP/1.1
Foo: bPOST /search HTTP/1.1
Host: example.com
```
If the attack is successful the victim request (`POST` and onwards) will get a 404 response.
#### TE.CL Socket Poisoning
The need for a closing chunk means we need to specify all the headers ourselves and place the victim request in the body. 
- Ensure the Content-Length in the prefix is slightly larger than the body:
```
POST /search HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: zchunked

96
GET /404 HTTP/1.1
X: x=1&q=smugging&x=
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 100

X=
0

POST /search HTTP/1.1
Host: example.com
```
If the site is live, another user's request may hit the poisoned socket before yours, which will make your attack fail and potentially upset the user. 
- As a result, this process often takes a few attempts, and on high-traffic sites may require thousands of attempts.
# Burp Smuggling Extension
For more advanced use watch the [video](https://portswigger.net/research/http-desync-attacks), and check out the [documentation](https://github.com/PortSwigger/http-request-smuggler).
1. Right click on a request and click 'Launch Smuggle probe'
2. Ensure 'permute: badsetupLF' is checked
3. Go to 'Target' tab of Burp (at the top)
4. Select the site you are querying
5. Go to the 'Contents' section of this site to see all requests
6. Go to the 'Issues' section to view any issues
7. Select 'Request 1' to see the issue with the first request
8. Right-click the request and select 'Smuggle attack'
9. A new window will pop up. If you scroll down, you'll see the `GET /hopefull404 HTTP/1.1` request
10. Select 'Attack' button at the bottom
11. Observe the 404 error, and a 200 status request previously with the payload in the data, click 'Halt' at the bottom to pause, then 'Configure' to exit
12. We can go back to the window where we saw the `GET` request section, and we can replace that with any other request we want
	- We can intercept other `GET` requests from other pages, go back to our smuggle request, and paste other `GET` requests into this section (only paste the `GET` and `HOST` section, and keep the `X-ignore` header when pasting)
13. We click 'Attack' again, and we can observe the request was successful
# References to Helpful Resources
## Identifying HTTP Request Smuggling Vulnerabilities
https://portswigger.net/web-security/request-smuggling/finding
## Exploiting HTTP request smuggling vulnerabilities
https://portswigger.net/web-security/request-smuggling/exploiting
## Advanced HTTP request smuggling
https://portswigger.net/web-security/request-smuggling/advanced
## Browser-powered request smuggling
https://portswigger.net/web-security/request-smuggling/browser
# How to prevent HTTP request smuggling vulnerabilities
HTTP request smuggling vulnerabilities arise in situations where the front-end server and back-end server use different mechanisms for determining the boundaries between requests. This may be due to discrepancies between whether HTTP/1 servers use the `Content-Length` header or chunked transfer encoding to determine where each request ends. In HTTP/2 environments, the common practice of [downgrading HTTP/2 requests](https://portswigger.net/web-security/request-smuggling/advanced/http2-downgrading) for the back-end is also fraught with issues and enables or simplifies a number of additional attacks.

To prevent HTTP request smuggling vulnerabilities, we recommend the following high-level measures:

- Use HTTP/2 end to end and disable HTTP downgrading if possible. HTTP/2 uses a robust mechanism for determining the length of requests and, when used end to end, is inherently protected against request smuggling. If you can't avoid HTTP downgrading, make sure you validate the rewritten request against the HTTP/1.1 specification. For example, reject requests that contain newlines in the headers, colons in header names, and spaces in the request method.
    
- Make the front-end server normalize ambiguous requests and make the back-end server reject any that are still ambiguous, closing the TCP connection in the process.
    
- Never assume that requests won't have a body. This is the fundamental cause of both CL.0 and client-side desync vulnerabilities.
    
- Default to discarding the connection if server-level exceptions are triggered when handling requests.
    
- If you route traffic through a forward proxy, ensure that upstream HTTP/2 is enabled if possible.
    

As we've demonstrated in the learning materials, disabling reuse of back-end connections will help to mitigate certain kinds of attack, but this still doesn't protect you from [request tunnelling](https://portswigger.net/web-security/request-smuggling/advanced/request-tunnelling) attacks.