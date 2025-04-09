# CTF Write-Up: Server-Side Template Injection (SSTI) to RCE

## Challenge Overview

This challenge required identifying a Server-Side Template Injection (SSTI) vulnerability and exploiting it to gain remote code execution (RCE), ultimately leading to flag extraction.

---

## Exploitation Process

### 1. Verified SSTI Vulnerability

Tested the input field using:

{{7+7}}

The application returned 14, confirming it was vulnerable to SSTI.
2. Identified Code Execution Path

Used a reference from the SSTI1 write-up to identify access to Python’s subprocess module.
3. Achieved Remote Code Execution

Constructed a payload leveraging subprocess.Popen to execute system commands through the template injection point.
4. Retrieved the Flag

Executed the following command through the template injection:

cat flag.txt

The flag was successfully displayed in the application response.
