# Recon Notes Checklist
## Note Structure Example
```
[Endpoint]: /admin  
- Requires authentication: Yes  
- Status Code: 401  
- Headers: Server: Apache/2.4.29  
- Notes: Add to brute-force list  

[Form]: /register  
- Fields: username, email, password  
- Input reflected in response: Yes  
- JavaScript validation: None  
```

### 1. Target Information
- IP address or domain name
- Technology stack (e.g., PHP, Node.js, WordPress)
- Web server and version (e.g., Apache, Nginx)
- CMS or framework (e.g., WordPress, Laravel, React)
- Interesting HTTP headers (e.g., `X-Powered-By`, `Server`, `Strict-Transport-Security`)
### 2. Discovered URLs/Endpoints (from FFUF or crawling)
- Full paths (e.g., `/admin`, `/login`, `/upload`, `/api/v1/users`)
- HTTP status codes (200, 403, 401, 500, etc.)
- Notes on interesting responses or error messages
- Authentication-related endpoints
- Upload, download, or file include endpoints
- API endpoints (especially undocumented ones)
- Hidden or backup files (e.g., `.git`, `backup.zip`, `config.old`)
### 3. Parameter Discovery
- GET/POST parameters (e.g., `?id=1`, `?page=home`)
- Hidden form fields
- Headers used (e.g., `X-Forwarded-For`, `Authorization`)
- File name or path parameters (LFI/RFI candidates)
- JSON or XML payload structures
### 4. Forms and Input Points
- Input forms (login, registration, search, comment, contact)
- File upload forms
- Input reflection in responses (potential for XSS or injection)
- Areas where user input is echoed or processed
### 5. Authentication and Access Control
- Login mechanisms and endpoints
- Session tokens (cookies, JWTs, tokens in local/sessionStorage)
- Role-based content or hidden features
- "Remember me" functionality
- Password reset functionality (check for username/email enumeration)
### 6. Client-Side JavaScript
- External and inline JavaScript files
- Obfuscated or minified JavaScript
- Client-side validation, encoding, or token logic
- Dynamically constructed endpoints (e.g., via `fetch()`, `XMLHttpRequest`)
### 7. Cookies and Storage
- All cookies and their attributes (`Secure`, `HttpOnly`, etc.)
- Local storage or session storage contents (especially tokens)
### 8. Error Messages and Debug Information
- Error messages disclosing file paths or source code
- Stack traces or verbose server responses
### 9. HTTP Headers and Server Responses
- CORS headers
- Cache control headers
- Content-Type headers
- Content-Security-Policy (CSP) headers
### 10. Miscellaneous
- `robots.txt` contents
- `sitemap.xml` contents
- Credentials or sensitive info in HTML comments or JS files
- Version disclosure via metadata or visible text (e.g., changelogs, footers)
