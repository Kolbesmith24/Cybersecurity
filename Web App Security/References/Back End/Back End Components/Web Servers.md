# Web Servers
A [web server](https://en.wikipedia.org/wiki/Web_server) is an application that runs on the back end server, which handles all of the HTTP traffic from the client-side browser, routes it to the requested pages, and finally responds to the client-side browser.
- Usually run on TCP [ports](https://en.wikipedia.org/wiki/Port_\(computer_networking\)) `80` or `443`
## Workflow
A typical web server accepts HTTP requests from the client-side, and responds with different HTTP responses and codes, like a code `200 OK` response for a successful request.
![Diagram showing a web server handling requests and responses from three clients: smartphone, laptop, and monitor.](https://academy.hackthebox.com/storage/modules/75/web-server-requests.jpg)

The following are some of the most common [HTTP response codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status):

|Code|Description|
|---|---|
|**Successful responses**||
|`200 OK`|The request has succeeded|
|**Redirection messages**||
|`301 Moved Permanently`|The URL of the requested resource has been changed permanently|
|`302 Found`|The URL of the requested resource has been changed temporarily|
|**Client error responses**||
|`400 Bad Request`|The server could not understand the request due to invalid syntax|
|`401 Unauthorized`|Unauthenticated attempt to access page|
|`403 Forbidden`|The client does not have access rights to the content|
|`404 Not Found`|The server can not find the requested resource|
|`405 Method Not Allowed`|The request method is known by the server but has been disabled and cannot be used|
|`408 Request Timeout`|This response is sent on an idle connection by some servers, even without any previous request by the client|
|**Server error responses**||
|`500 Internal Server Error`|The server has encountered a situation it doesn't know how to handle|
|`502 Bad Gateway`|The server, while working as a gateway to get a response needed to handle the request, received an invalid response|
|`504 Gateway Timeout`|The server is acting as a gateway and cannot get a response in time|
Web servers also accept various types of user input within HTTP requests, including text, [JSON](https://www.w3schools.com/js/js_json_intro.asp), and even binary data (i.e., for file uploads). Once a web server receives a web request, it is then responsible for routing it to its destination, run any processes needed for that request, and return the response to the user on the client-side.

We can even develop our own basic web server using languages such as `Python`, `JavaScript`, and `PHP`. However, for each language, there's a popular web application that is optimized for handling large amounts of web traffic, which saves us time in creating our own web server.
## Apache
[Apache](https://www.apache.org/) 'or `httpd`' is the most common web server on the internet.
- Usually comes pre-installed in most `Linux` distributions and can also be installed on Windows and macOS servers.

`Apache` is usually used with `PHP` for web application development, but it also supports other languages like `.Net`, `Python`, `Perl`, and even OS languages like `Bash` through `CGI`. Users can install a wide variety of `Apache` modules to extend its functionality and support more languages.
- Ex: to support serving `PHP` files, users must install `PHP` on the back end server, in addition to installing the `mod_php` module for `Apache`.
## NGINX
The second most common web server on the internet.

Focuses on serving many concurrent web requests with relatively low memory and CPU load by utilizing an async architecture to do so
## IIS
[IIS (Internet Information Services)](https://en.wikipedia.org/wiki/Internet_Information_Services) is the third most common web server on the internet.

Developed and maintained by Microsoft and mainly runs on Microsoft Windows Servers. `IIS` is usually used to host web applications developed for the Microsoft .NET framework, but can also be used to host web applications developed in other languages like `PHP`, or host other types of services like `FTP`. 

Furthermore, `IIS` is very well optimized for Active Directory integration and includes features like `Windows Auth` for authenticating users using Active Directory, allowing them to automatically sign in to web applications.

## Others 
Aside from these 3 web servers, there are many other commonly used web servers, like [Apache Tomcat](https://tomcat.apache.org/) for `Java` web applications, and [Node.JS](https://nodejs.org/en/) for web applications developed using `JavaScript` on the back end.