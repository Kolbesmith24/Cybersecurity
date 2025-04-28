# Steve Le Poisson Writeup - UMDCTF 2025

## Reconnaissance
Upon visiting the provided website, we observe that it plays a video and displays the lyrics to a French song titled **"Steve the Fish"**.

![image](https://github.com/user-attachments/assets/e1961152-38db-4c6c-b274-06073883c48a)

The page includes an input field, prompting the user to guess "what Steve is."

When entering a **normal character** (letters or digits), the site responds with a typical "Well, you're wrong" message. However, when submitting a **special character** (such as `!@#%`), the application returns a much longer and dramatically worded error message:

![image](https://github.com/user-attachments/assets/e95e85b1-a9da-4100-96ba-d24cfb2893dd)

This verbose message — when translated — reveals that the input field expects only **alphabetical** and **numerical** characters, criticizing users for entering special symbols.  
Importantly, it also hints at the existence of an HTTP header named **`X-Steve-Supposition`**.

---

Inspecting the browser’s **Network** tab, we observe that the **`X-Steve-Supposition` header** reflects **whatever input** we submit into the form:

![image](https://github.com/user-attachments/assets/8c26e488-fa5b-46a4-89ae-c336c7ca472e)

If no special header is submitted when using Burp Suite, the site returns another theatrical complaint about the missing `X-Steve-Supposition` header, emphasizing its importance:

> _"This header is fundamental — it’s where you declare your assumptions, your intentions, your respect for the sacred protocol of Steve."_

At this point, it becomes clear that **manipulating this custom header** is likely key to progressing further.

---

## Exploitation
Initially, attempts to tamper with the header — including sending basic payloads via Burp Suite — were unsuccessful. The server expected properly formatted input and would reject malformed guesses.

**Brute-forcing** every possible input was considered but deemed **too slow and inefficient**.

During testing, it was noticed that sending **too many headers** (e.g., dozens of custom headers) caused another custom error message to trigger, criticizing the "verbosity" of the HTTP request:

> _"Every request you send is like a novel... And what’s with all these duplicate headers?"_

This gave an important clue: **the server is parsing multiple `X-Steve-Supposition` headers** — and importantly, it **only processes the **last** one** due to how **regular expressions (regex)** are being used on the backend.

---

Further analysis of the **provided `index.js` file** confirmed the backend uses **SQLite** for its database:

![image](https://github.com/user-attachments/assets/55605d77-44b1-46f9-92ad-75b973e5879b)

This strongly suggested that **SQL injection** could be possible.

With this, we come to the conclusion that:
- The application uses **regex** to validate user input based on the **last** `X-Steve-Supposition` header.
- **SQLite** databases are often vulnerable to classic **blind SQL injection** techniques if proper sanitation is missing.
- HTTP standards allow **multiple headers with the same name**; in Node.js, by default, duplicate headers are merged into an array, so **regex checking might only be applied to one value**.

---
### The Attack Plan:
By **sending two `X-Steve-Supposition` headers**:
1. The **first header** contains a **normal value** (like a letter) that **passes regex validation**.
2. The **second header** carries an **SQL injection payload** that the server later uses unsafely.

Thus, the injection bypasses input validation **and** reaches the database.

---
### Payload Used:
```http
X-Steve-Supposition: ' OR SUBSTR((SELECT * FROM flag LIMIT 1), 1, 7) = 'UMDCTF{' -- 
X-Steve-Supposition: steve
```

Here’s what it does:
- `' OR SUBSTR((SELECT * FROM flag LIMIT 1), 1, 7) = 'UMDCTF{' --` :
    - It checks if the **first 7 characters** of the flag match `'UMDCTF{'`.
    - If true, the SQL query becomes valid and returns a successful response.
    - The comment `--` ensures that any SQL syntax following the payload is ignored.

**Note:**  
The **first header** (`steve`) passes regex validation, satisfying the front-end, while the **second header** actually executes SQL on the backend.

---
### Final Result:
Submitting the crafted request via Burp Suite returned the flag:

```
UMDCTF{ile5TVR4IM3NtTresbEAu}
```

---
