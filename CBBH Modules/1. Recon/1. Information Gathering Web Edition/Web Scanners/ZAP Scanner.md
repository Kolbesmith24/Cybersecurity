# ZAP Scanner
ZAP includes a web scanner like Burp Suite Pro, capable of spidering, passive scanning, and active scanning.
## Spider (Site Crawler)
- **How to Start:**
    - Right-click a request in the _History tab_ → `Attack > Spider`.
    - Or use the HUD: Click the **Spider button** (2nd on right pane).
- **Scope:**  
    ZAP may prompt to add the target to scope—click **Yes**.
- **Results:**  
    Progress shown in _Spider tab_. Found URLs appear in the _Sites Tree_ (first button on right pane).
- **Ajax Spider:**  
    Use the **third button** on the right pane. It finds JS/AJAX-loaded links missed by the normal spider.
### Passive Scanner
- Runs automatically during Spidering.
- Detects issues like missing headers or DOM-based XSS without sending malicious payloads.
- View issues under the _Alerts tab_ (left: current page, right: all pages).
## Active Scanner
- Start via the **Active Scan** button (right pane).
- If the site tree is empty, it will run a Spider first.
- Sends attack payloads to parameters and pages to uncover vulnerabilities.
- Progress is tracked in real time; more _alerts_ appear as issues are found.
- Focus on **High** severity alerts—these may lead to full compromise.

### 📄 Reporting
- Generate reports via:  
    `Report > Generate HTML Report` (or export as XML/Markdown).
- Reports include all findings in an organized format—great for documentation.