# **Burp Scanner Overview**

Burp Scanner (Pro only) is a powerful tool in Burp Suite for web vulnerability scanning, using a **Crawler** (to map the site) and **Scanner** (for passive/active analysis). It’s not available in the Community Edition.

---
### **Starting a Scan**
You can start scans from:
- **Proxy History** (right-click > Scan)
- **Dashboard** > _New Scan_
- **Items in Target Scope**

Use **Target > Site map** to see discovered directories/files. Right-click items to **Add/Remove from scope**. Scoping is key to limiting scans and avoiding logout or dangerous actions.

Use **Target > Scope** for advanced regex control over included/excluded items.

---

### **Crawler**
In the **Dashboard > New Scan**, choose:
- **Crawl only** – maps links and forms
- **Crawl and Audit** – maps + scans for vulnerabilities

Crawl doesn’t brute-force hidden paths (use Intruder or Content Discovery for that). Use **Select from library** for preset scan configs (e.g., _Crawl strategy - fastest_). Optionally, provide **login credentials or record login steps** to enable authenticated scans.

Progress appears in the Dashboard’s **Tasks** pane. Click **View details** for scan status and config options.

---

### **Passive Scan**
Passive scans analyze traffic **without sending new requests**—useful for catching issues like missing headers or DOM-based XSS. Right-click in Proxy History or Site Map > **Do passive scan**.

Check results in **Dashboard > Issue activity**. Focus on **High severity** with **Certain/Firm confidence**, especially for sensitive apps.

---

### **Active Scan**
The most comprehensive scan:
1. Crawls and maps the site
2. Runs a Passive Scan
3. Sends requests to **verify findings**
4. Analyzes JavaScript
5. **Fuzzes parameters** for XSS, SQLi, Command Injection, etc.

Start it like a passive scan or via **Dashboard > New Scan > Crawl and Audit**. Configure:
- **Crawl settings**
- **Audit checks** (e.g., _critical issues only_)
- **Login creds**

Scan duration varies by scope/config. View active scan logs in **Logger** tab. Results are again shown in **Issue activity**, where you can filter by severity/confidence.

---

### **Reporting**
After scanning, use the **Issue activity** pane to review and prioritize vulnerabilities. Burp provides detailed advisories, request/response data, and remediation info for each issue.