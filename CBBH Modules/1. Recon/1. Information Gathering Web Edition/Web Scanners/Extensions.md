# Extensions in Burp & ZAP 
## **Burp Suite**
- **Extensions via:** `Extender` tab → `BApp Store`
- **Types:** Some free, some for Pro users only.
- **Popular Extensions:**
    - `Decoder Improved` – Adds extra encoders/hashers (e.g., MD5)
    - `.NET Beautifier`, `J2EEScan`, `Active Scan++`, `CMS Scanner`, `Autorize`, `Retire.js`, `Backslash Powered Scanner`, `CSP Auditor`, `CSRF Scanner`

- **Requirements:** Some extensions need `Jython` or other dependencies.
- **Docs:** Each extension has documentation and/or GitHub link for usage.
## **ZAP (Zed Attack Proxy)**
- **Extensions via:** `Manage Add-ons` → `Marketplace` tab
- **Build Types:** Release (stable), Beta/Alpha (experimental)
- **Recommended Add-ons:**
    - `FuzzDB Files` & `FuzzDB Offensive` – Adds large wordlist sets for fuzzing (e.g., OS command injection payloads)

- **Use Case Example:** Use `fuzzdb>attack>os-cmd-execution` in fuzzer for command injection discovery.