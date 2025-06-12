Whenever web applications need to transfer files or move the user parameters from the URL, they utilize `POST` requests.

Where can they be used?
## Login Forms
```bash
curl -X POST -d 'username=admin&password=admin' https://example.com/upload
```
- `-X` sets the HTTP method (GET, POST, etc.)
	- `-d` adds the POST data
### Authenticated Cookies
If we were successfully authenticated, we should have received a cookie so our browsers can persist our authentication, and we don't need to login every time we visit the page.
- Use the `-v` or `-i` flags to view the response, which should contain the `Set-Cookie` header or whatever the cookie header is

We can then change that cookie in `Storage` in developer tools and reload the page to authenticate with that cookie
## JSON Data
If the POST data appears to be in JSON format, our request must have specified the `Content-Type` header to be `application/json`.
- We can confirm this by right-clicking on the request, and selecting `Copy>Copy Request Headers`.

We can replicate this request as we did earlier, but include both the cookie and content-type headers, and send our request to the destination.