# Quick Guide to `curl` in Linux

`curl` is a powerful command-line tool used to send and receive data from URLs. It's often used to interact with APIs, download files, or test network connections.

---

## Basic Syntax

```bash
curl [URL]
```

Use `curl` followed by options (like `-o`, `-X POST`, etc.) and the URL you want to interact with.

---

## Common Examples

### 1. View a Web Page in Terminal
```bash
curl https://example.com
```
This shows the HTML content of a webpage in your terminal.

---

### 2. Download a File
```bash
curl -O https://example.com/file.zip
```
- `-O` saves the file with its original name from the server.

---

### 3. Save Output to a Specific File
```bash
curl https://example.com -o saved.html
```
- `-o [filename]` lets you name the file you’re saving the output to.

---

### 4. Skip Certificate Check
```bash
curl -K https://bit.ly/some-link
```
- `-K` ensures `curl` still goes through on web applications that may not yet have implemented a valid SSL certificate

---

### 5. View full HTTP Requests and Responses
```bash
curl -I https://bit.ly/some-link
```
- `-I` sends a `HEAD` request and only displays the response headers
	- `-i` displays both the headers and the response body (HTML code)

---

### 6. Add Request Headers
```bash
curl -H 'Authorization: Basic' https://bit.ly/some-link
```
- Can add specific headers:
	- `-A` sets user agent

---

### 7. Provide Credentials Through cURL
```bash
curl -u admin:password https://bit.ly/some-link
```
- `-u` to provide the credentials through cURL

---

### 8. Define the Request Type
```bash
curl -X POST -d 'username=admin&password=admin' https://example.com/upload
```
- `-X` sets the HTTP method (GET, POST, etc.)
	- `-d` adds the POST data

---

### 9. Set A Cookie For the Reques
```bash
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' https://example.com
```
- `-v` (verbose) shows all the behind-the-scenes connection details.
	- Can also specify cookie as a header with `-H`

---

## Handy Options Summary

| Option | What it Does                            |
| ------ | --------------------------------------- |
| `-o`   | Save output to a specific file          |
| `-O`   | Save using the original filename        |
| `-L`   | Follow redirects                        |
| `-H`   | Add a custom header                     |
| `-d`   | Send POST data                          |
| `-X`   | Set HTTP method (GET, POST, etc.)       |
| `-F`   | Upload files via form fields            |
| `-u`   | Basic authentication (`-u user:pass`)   |
| `-v`   | Show detailed output                    |
| `-s`   | Silent mode (no progress bar or errors) |
| `-b`   | Sets the cookie for the request         |

---

## Learn More

Run these in your terminal for more info:

```bash
man curl
curl --help
```
