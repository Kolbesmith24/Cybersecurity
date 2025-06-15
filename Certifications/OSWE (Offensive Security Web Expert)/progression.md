# Study Guide
To study for the OSWE, I am first enumerating through all the boxes provided by HackTheBox that are targeted towards the OSWE using both TJNull's list and a collection of boxes revolving around web based exploitation:
[TJNull's OSWE HTB List](https://docs.google.com/spreadsheets/u/0/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/htmlview?pli=1#)
- I added a few extra boxes to improve my web exploitation skills in general for CTF competitions!

I am also planning on writing a script for each box to practice my scripting skills. That being said, I will also be enumerating through W3 schools and going through the Python, JavaScript, PHP, and C modules.
https://www.w3schools.com/

I am also utilizing PortSwigger Academy course materials to learn all web exploitation techniques.

Once I've completed all PortSwigger Academy Topics, and all the boxes and challenges from HTB and learned all techniques from each box, I am planning on taking practice exams to dial in my knowledge and focus on my weak areas.

Follow my progression as I post writeups for each box I complete [here](/Writeups/Hack%20The%20Box/).

## W3 Schools Scripting Languages 
- *Go through entire course for each language*
- [x] Python
- [ ] JavaScript
- [ ] C
- [ ] PHP

---

## PortSwigger Academy Topics
### Server-Side Topics
- [ ]  **Access control** 🔄
- [ ]  **API testing**
- [ ]  **Authentication** 🔄
- [x]  **Command injection** ✅
- [ ]  **File upload vulnerabilities** 🔄
- [ ]  **Information disclosure**
- [ ]  **Server-side request forgery (SSRF)** 🔄
- [x]  **SQL injection** ✅
- [ ]  **XXE injection**
- [ ]  NoSQL injection
- [ ]  Race conditions
- [x]  **Path traversal** ✅
- [ ]  **Business logic vulnerabilities**
- [ ]  _Web cache deception_
### Client-Side Topics
- [ ]  _Clickjacking_
- [ ]  _Cross-origin resource sharing (CORS)_
- [ ]  Cross-site request forgery (CSRF)
- [ ]  _DOM-based vulnerabilities_
- [ ]  Cross-site scripting (XSS) 🔄
- [ ]  _WebSockets_
### Advanced Topics
- [ ]  _GraphQL API vulnerabilities_ 
- [ ]  HTTP Host header attacks
- [ ]  HTTP request smuggling 🔄
- [ ]  **Insecure deserialization**
- [ ]  JWT attacks 🔄
- [ ]  OAuth authentication
- [ ]  **Prototype pollution**
- [ ]  **Server-side template injection** 🔄
- [ ]  _Web cache poisoning_
- [ ]  _Web LLM attacks_

_Note:_
```
Bolded = Higher Importance
Nothing = Medium Importance
Italisized = Low Importance

Checked Box & ✅ = Finished
🔄 = In Progress 
```

---
## HTB Web Challenges for OSWE/CTFs:
### 🟢 Easy
- [x] **Flag Command** – Command injection vulnerability.
- [x] **Spookifier** - SSTI
- [x] **Pentest Notes**: SQLi → RCE
- [ ] **Breathtaking View**: SSTI in Java → RCE
- [ ] **Interstellar**: SQLi → Web shell (RCE)
- [ ] **DoxPit**: SSRF and SSTI chain
- [ ] **JerryTok**: Exploiting Tomcat (web app misconfig, leads to RCE)
- [ ] **POP Restaurant** – PHP Object Injection
- [ ] **PDFy** – SSRF
- [x] **Insomnia** – Authentication Bypass via Source Code Analysis
- [ ] **Trapped Source** – Bug discovered through source code review.
- [ ] **Templated** – Template injection vulnerability.
- [ ] **baby auth** – Authentication bypass issue.
- [ ] **HTBank** – Authentication bypass combined with logic issues.
- [ ] **ProxyAsAService** – SSRF vulnerability and potential HTTP smuggling.
- [ ] **CurlAsAService** – SSRF vulnerability.
- [ ] **ApacheBlaze** – Apache misconfigurations and SSRF vulnerabilities.
### 🟡 Medium
- [ ] **SerialFlow** – Insecure deserialization vulnerability.
- [ ] **WS-Todo** – Business logic flaw.
- [ ] **GrandMonty** – Known box with complex logic issues.
- [ ] **baby sql** – SQL injection, particularly blind SQLi.
- [ ] **nginxatsu** – Path traversal and filter bypass.
- [ ] **breaking grad** – Exploit chaining involving multiple vulnerabilities.
- [ ] **wafwaf** – WAF bypass combined with injection.
- [ ] **interdimensional internet ** – Chained logic bypass vulnerabilities.
- [ ] **pcalc** – Expression evaluation leading to RCE.
- [ ] **baby website rick** – Logic flaws and chaining vulnerabilities.
### 🟠 Hard
- [ ] **Pod Diagnostics** – Complex chained vulnerabilities.
- [ ] **ArtificialUniversity** – Source code review and bypass chaining.
- [ ] **BoneChewerCon** – Multiple OSWE-style logic flaws.
- [ ] **ImageTok** – File upload vulnerability combined with deserialization.
### 🔴Insane
- [ ] **ArtificialUniversity** – Advanced Web Exploitation

---
## HTB Boxes for OSWE/CTFs:
### 🟢 **Easy**
- [x] **Academy** – Basic IDOR and OAuth misconfigs
- [ ] **Previse** – Race condition and path manipulation
- [ ] **Laboratory** – Prototype pollution is often tested in modern web app assessments.
- [ ] **MonitorsTwo** – Auth bypass + command injection
- [ ] **Trick** – Multi-stage auth bypass and RCE
- [ ] **Help** – XSS for chained attacks (e.g., XSS to token theft).

### 🟡 **Medium**
- [ ] **Book** – SQLi and XSS, especially front-end sanitization
- [ ] **Magic** – File upload bypass and weak validation
- [ ] **Lazy** – File upload to RCE
- [ ] **Mango** – NoSQL injection and session logic flaws
- [ ] **Craft** – SSRF to RCE through API abuse
- [ ] **Vault** – Auth bypass and race conditions
- [ ] **Catch** – Regex filter bypass aka filter evasion
- [ ] **Celestial** – Prototype pollution and logic flaws
### 🟠 **Hard**
- [ ] **ForwardSlash** – Path traversal and logic flaws
- [ ] **Zipper** – Path traversal via ZIP
- [ ] **Cereal** – Deserialization attacks
- [ ] **Registry** – File upload + SQLi
- [ ] **Falafel** – SQLi via manual exploitation
- [ ] **Unobtainium** – Auth bypass and multi-stage exploitation
### 🔴 **Insane**
- [ ] **CrossFitTwo** – Advanced logic flaws and web priv esc
- [ ] **Sink** – Dynamic feature abuse leading to RCE
- [ ] **Crossfit** – Misconfigured endpoints and auth bypass
- [ ] **Stacked** – Chained vulnerabilities in web apps
- [ ] **Fulcrum** – Chaining web flaws.

---
