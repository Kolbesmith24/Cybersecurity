# Quick Guide to `curl` in Linux

`curl` is a powerful command-line tool used to send and receive data from URLs. It's often used to interact with APIs, download files, or test network connections.

---

## Basic Syntax

```bash
curl [options] [URL]
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

### 4. Follow Redirects Automatically
```bash
curl -L https://bit.ly/some-link
```
- `-L` ensures `curl` follows any redirection the URL might use.

---

### 5. Add Custom Headers (e.g. for APIs)
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" https://api.example.com/data
```
- `-H` adds headers to the request, like auth tokens.

---

### 6. Send a POST Request with Form Data
```bash
curl -X POST -d "username=admin&password=1234" https://example.com/login
```
- `-d` sends form data.
- `-X POST` sets the HTTP method to POST.

---

### 7. Send JSON in a POST Request
```bash
curl -X POST -H "Content-Type: application/json" \
-d '{"name":"Kolbe","role":"pentester"}' \
https://api.example.com/users
```
- Use this for interacting with REST APIs that expect JSON.

---

### 8. Upload a File
```bash
curl -F "file=@image.jpg" https://example.com/upload
```
- `-F` sends files as multipart form data (like in web forms).

---

### 9. Show Detailed Request and Response Info
```bash
curl -v https://example.com
```
- `-v` (verbose) shows all the behind-the-scenes connection details.

---

## Handy Options Summary

| Option | What it Does |
|--------|---------------|
| `-o`   | Save output to a specific file |
| `-O`   | Save using the original filename |
| `-L`   | Follow redirects |
| `-H`   | Add a custom header |
| `-d`   | Send POST data |
| `-X`   | Set HTTP method (GET, POST, etc.) |
| `-F`   | Upload files via form fields |
| `-u`   | Basic authentication (`-u user:pass`) |
| `-v`   | Show detailed output |
| `-s`   | Silent mode (no progress bar or errors) |

---

## Learn More

Run these in your terminal for more info:

```bash
man curl
curl --help
```

Would you like me to save this as a `.md` file and send you a download link?
