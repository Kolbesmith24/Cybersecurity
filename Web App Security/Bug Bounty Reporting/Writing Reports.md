# Writing a Good Report
Clear, concise reports help triage and security teams understand and reproduce vulnerabilities quickly. Tailor language for less technical companies to explain the business impact.
## Key Components of a Good Report:
- **Title**: Summarize vuln type, affected domain/parameter/endpoint, and potential impact.
- **CWE & CVSS Score**: Standardized ways to describe the weakness and its severity.
- **Description**: Explain what caused the vulnerability.
- **Proof of Concept (PoC)**: Clear steps to reproduce the issue.
- **Impact**: Describe what an attacker can do if they fully exploit the issue (including business damage).
- **Remediation (Optional)**: Suggest a fix.

Well-structured reports reduce triage and reproduction time significantly.

---
## CWE & CVSS Overview
- **CWE**: Standardized identifiers for common vulnerability types (by MITRE).
- **CVSS (v3.1)**: Used to assess and communicate severity using a Base Score (ignore Temporal & Environmental for most bug bounties).

### CVSS Base Metrics (with key options):
- **Attack Vector**:
    - N (Network), A (Adjacent), L (Local), P (Physical)
- **Attack Complexity**:
    - L (Low), H (High)
- **Privileges Required**:
    - N (None), L (User), H (Admin)
- **User Interaction**:
    - N (None), R (Required)
- **Scope**:
    - U (Unchanged), C (Changed)
- **Confidentiality / Integrity / Availability**:
    - N (None), L (Low), H (High)

Use the CVSS v3.1 Calculator to generate the score: https://www.first.org/cvss/calculator/3.1

---

### 📄 Good Report Examples (HackerOne)

- [SSRF in Exchange leads to ROOT access in all instances](https://hackerone.com/reports/341876)
- [Remote Code Execution in Slack desktop apps + bonus](https://hackerone.com/reports/783877)
- [Full name of other accounts exposed through NR API Explorer (another workaround of #476958)](https://hackerone.com/reports/520518)
- [A staff member with no permissions can edit Store Customer Email](https://hackerone.com/reports/980511)
- [XSS while logging in using Google](https://hackerone.com/reports/691611)
- [Cross-site Scripting (XSS) on HackerOne careers page](https://hackerone.com/reports/474656)
## Interacting with Organizations/BBP **Hosts**
If the security/triage team does not get back to you in a reasonable amount of time, then if the submission was through a bug bounty platform, you can contact [Mediation](https://docs.hackerone.com/hackers/hacker-mediation.html).

During your interaction with the security/triage team, there could be disagreements about the severity of the bug or the bounty. A bug's impact and severity play a significant role during the bounty amount assignment. In the case of such a disagreement, proceed as follows.

- Explain your rationale for choosing this severity score and guide the security/triage team through each metric value you specified in the CVSS calculator. Eventually, you will come to an agreement.
- Go over the bug bounty program's policy and scope and ensure that your submission complies with both. Also, make sure that the bounty amount resembles the policy of the bug bounty program.
- If none of the above was fruitful, contact mediation or a similar platform service.

# Example 1: Reporting Stored XSS
---
`Title`: Stored Cross-Site Scripting (XSS) in X Admin Panel
`CWE`: [CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')](https://cwe.mitre.org/data/definitions/79.html)
`CVSS 3.1 Score`: 5.5 (Medium)
`Description`: During our testing activities, we identified that the "X for administrators" web application is vulnerable to stored cross-site scripting (XSS) attacks due to inadequate sanitization of user-supplied data. Specifically, the file uploading mechanism at "Admin Info" -> "Secure Data Transfer" -> "Load of Data" utilizes a value obtained from user input, specifically the uploaded file's filename, which is not only directly reflected back to the user’s browser but is also stored into the web application’s database. However, this value does not appear to be adequately sanitized. It, therefore, results in the application being vulnerable to reflected and stored cross-site scripting (XSS) attacks since JavaScript code can be entered in the filename field.

`Impact`: Cross-Site Scripting issues occur when an application uses untrusted data supplied by offensive users in a web browser without sufficient prior validation or escaping. A potential attacker can embed untrusted code within a client-side script to be executed by the browser while interpreting the page. Attackers utilize XSS vulnerabilities to execute scripts in a legitimate user's browser leading to user credentials theft, session hijacking, website defacement, or redirection to malicious sites. Anyone that can send data to the system, including administrators, are possible candidates for performing XSS attacks against the vulnerable application. This issue introduces a significant risk since the vulnerability resides in the "X for administrators” web application, and the uploaded files are visible and accessible by every administrator. Consequently, any administrator can be a possible target of a Cross-Site Scripting attack.

`POC`:

Step 1: A malicious administrator could leverage the fact that the filename value is reflected back to the browser and stored in the web application’s database to perform cross-site scripting attacks against other administrators by uploading a file containing malicious JavaScript code into its filename. The attack is feasible because administrators can view all uploaded files regardless of the uploader. Specifically, we named the file, as follows, using a Linux machine.

Code: javascript

```javascript
"><svg onload = alert(document.cookie)>.docx
```

![File upload interface showing a potentially malicious file.](https://academy.hackthebox.com/storage/modules/161/2.png)

Step 2: When another administrator clicks the view button to open the abovementioned file, the malicious JavaScript code in the file’s filename will be executed on the browser.

![Admin interface showing uploaded files, including a highlighted file named 'malicious_filename' uploaded by Pentest Admin01.](https://academy.hackthebox.com/storage/modules/161/3.png) ![Admin interface with a session alert popup displaying session details for Pentest Admin02.](https://academy.hackthebox.com/storage/modules/161/4.png)

---

## CVSS Score Breakdown
- `Attack Vector:`Network - The attack can be mounted over the internet. 
 - `Attack Complexity:` Low - All the attacker (malicious admin) has to do is specify the XSS payload eventually stored in the database. 
 - `Privileges Required:` High - Only someone with admin-level privileges can access the admin panel. 
 - `User Interaction:` None - Other admins will be affected simply by browsing a specific (but regularly visited) page within the admin panel. 
 - `Scope:` Changed - Since the vulnerable component is the webserver and the impacted component is the browser 
 - `Confidentiality:` Low - Access to DOM was possible 
 - `Integrity:` Low - Through XSS, we can slightly affect the integrity of an application 
 - `Availability:` None - We cannot deny the service through XSS
# Example 2: Reporting CSRF
---
`Title`: Cross-Site Request Forgery (CSRF) in Consumer Registration
`CWE`: [CWE-352: Cross-Site Request Forgery (CSRF)](https://cwe.mitre.org/data/definitions/352.html)
`CVSS 3.1 Score`: 5.4 (Medium)
`Description`: During our testing activities, we identified that the web page responsible for consumer registration is vulnerable to Cross-Site Request Forgery (CSRF) attacks. Cross-Site Request Forgery (CSRF) is an attack where an attacker tricks the victim into loading a page that contains a malicious request. It is malicious in the sense that it inherits the identity and privileges of the victim to perform an undesired function on the victim's behalf, like change the victim's e-mail address, home address, or password, or purchase something. CSRF attacks generally target functions that cause a state change on the server but can also be used to access sensitive data.

`Impact`: The impact of a CSRF flaw varies depending on the nature of the vulnerable functionality. An attacker could effectively perform any operations as the victim. Because the attacker has the victim's identity, the scope of CSRF is limited only by the victim's privileges. Specifically, an attacker can register a fintech application and create an API key as the victim in this case.

`POC`:

Step 1: Using an intercepting proxy, we looked into the request to create a new fintech application. We noticed no anti-CSRF protections being in place.

![Application registration form with fields for application type, name, description, and developer email.](https://academy.hackthebox.com/storage/modules/161/5.png) ![HTTP POST request to /consumer-registration with parameters for app type, name, developer email, and description.](https://academy.hackthebox.com/storage/modules/161/6.png)

Step 2: We used the abovementioned request to craft a malicious HTML page that, if visited by a victim with an active session, a cross-site request will be performed, resulting in the advertent creation of an attacker-specific fintech application.

![HTTP request with CSRF HTML form showing POST to /consumer-registration with parameters for app type, name, developer email, and description.](https://academy.hackthebox.com/storage/modules/161/7.png)

Step 3: To complete the attack, we would have to send our malicious web page to a victim having an open session. The following image displays the actual cross-site request that would be issued if the victim visited our malicious web page.

![HTTP POST request to /consumer-registration with parameters for app type, name, developer email, and description.](https://academy.hackthebox.com/storage/modules/161/8.png)

Step 4: The result would be the inadvertent creation of a new fintech application by the victim. It should be noted that this attack could have taken place in the background if combined with finding 6.1.1. <-- 6.1.1 was an XSS vulnerability.

![API registration confirmation showing application type 'Web', name 'Unwanted_FinTech App', developer email 'j_irons@gmail.com', and consumer keys.](https://academy.hackthebox.com/storage/modules/161/9.png)

---

## CVSS Score Breakdown
 - `Attack Vector:` Network - The attack can be mounted over the internet. 
 - `Attack Complexity:` Low - All the attacker has to do is trick a user that has an open session into visiting a malicious website. 
 - `Privileges Required:` None - The attacker needs no privileges to mount the attack. 
 - `User Interaction:` Required - The victim must click a crafted link provided by the attacker. 
 - `Scope:` Unchanged - Since the vulnerable component is the webserver and the impacted component is again the webserver. 
 - `Confidentiality:` Low - The attacker can create a fintech application and obtain limited information. 
 - `Integrity:` Low - The attacker can modify data (create an application) but limitedly and without seriously affecting the vulnerable component's integrity. 
 - `Availability:` None - The attacker cannot perform a denial-of-service through this CSRF attack. 