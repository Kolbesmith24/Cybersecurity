# HTTP Verb Tampering
An HTTP Verb Tampering attack exploits web servers that accept many HTTP verbs and methods. This can be exploited by sending malicious requests using unexpected methods, which may lead to bypassing the web application's authorization mechanism or even bypassing its security controls against other web attacks.

HTTP has [9 different verbs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) that can be accepted as HTTP methods by web servers:

|Verb|Description|
|---|---|
|`HEAD`|Identical to a GET request, but its response only contains the `headers`, without the response body|
|`PUT`|Writes the request payload to the specified location|
|`DELETE`|Deletes the resource at the specified location|
|`OPTIONS`|Shows different options accepted by a web server, like accepted HTTP verbs|
|`PATCH`|Apply partial modifications to the resource at the specified location|
### Insecure Configurations
A web server's authentication configuration may be limited to specific HTTP methods, which would leave some HTTP methods accessible without authentication.
### Insecure Coding
This can occur when a web developer applies specific filters to mitigate particular vulnerabilities while not covering all HTTP methods with that filter.

Example:
```php
$pattern = "/^[A-Za-z\s]+$/";

if(preg_match($pattern, $_GET["code"])) {
    $query = "Select * from ports where port_code like '%" . $_REQUEST["code"] . "%'";
    ...SNIP...
}
```
- Sanitization filter is only being tested on the `GET` parameter. If the GET requests do not contain any bad characters, then the query would be executed.
- In this case, an attacker may use a `POST` request to perform SQL injection
## Bypassing Basic Authentication
### Identify
If we examine an HTTP request after clicking the a button or look at the URL that the button navigates to after clicking it, we can see that it is at a certain destination (`/admin/reset.php`). So, either the `/admin` directory is restricted to authenticated users only, or only the `/admin/reset.php` page is.
### Exploit
We need to identify the HTTP request method used by the web application. We can intercept the request in Burp Suite and examine it.

As the page uses a `GET` request, we can send a `POST` request and see whether the web page allows `POST` requests.
- To do so, we can right-click on the intercepted request in Burp and select `Change Request Method`, and it will automatically change the request into a `POST` request.

If the server still responds with asking for credentials or says unauthorized, , it seems like the web server configurations do cover both `GET` and `POST` requests. Now we can try other methods, such as the `HEAD` method, which is identical to a `GET` request but does not return the body in the HTTP response.
- **If this is successful, we may not receive any output, but the button should still get executed**

To see whether the server accepts `HEAD` requests, we can send an `OPTIONS` request to it and see what HTTP methods are accepted, as follows:
```bash
curl -i -X OPTIONS http://SERVER_IP:PORT/
```

We can analyze what is under `Allow:` and try intercepting and sending requests with different verbs.

We can then try all kinds of verbs to see what works.
## Bypassing Security Filters
### Identify
We might try to upload a file to a file upload function with malicious characters such as the filename `test;`. 

If there is a message that says 'Malicious Request Denied', it shows the web application uses certain filters on the back-end to identify injection attempts and then blocks any malicious requests.
### Exploit
To try and exploit this vulnerability, let's intercept the request in Burp Suite (Burp) and then use `Change Request Method` to change it to another method.

If we don't get the malicious request response, we need to attempt exploiting the vulnerability the filter is protecting.
#### Example
A Command Injection vulnerability, in this case is protecting against malicious files being uploaded. So, we can inject a command that creates two files and then check whether both files were created. To do so, we will use the following file name in our attack (`file1; touch file2;`).

Then, we can once again change the request method to what worked in the previous step.
- Once we send our request, we see that this time both `file1` and `file2` were created.

This shows that we successfully bypassed the filter through an HTTP Verb Tampering vulnerability and achieved command injection.