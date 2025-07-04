## Categories of Authentication
- Knowledge: passwords, PINs, ...
- Ownership: ID cards, TOTP
- Inherence: Biometric authentication
## Brute-Force Attacks
- User Enumeration[[CBBH Modules (For Testing)/1. Recon/3. Brute Forcing/Wordlists#Username Anarchy]]
- Brute-Forcing Passwords[[CBBH Modules (For Testing)/1. Recon/3. Brute Forcing/Using Hydra]]
- Brute-Forcing Password Reset Tokens
- Brute-Forcing 2FA Codes
- Bypassing Brute-Force Protection
    - Rate Limit: `X-Forwarded-For` HTTP Header
    - CAPTCHAs: Look for CAPTCHA solution in HTML code
## Password Attacks
- Default Credentials
    - [CIRT.net](https://www.cirt.net/passwords)
    - [SecLists Default Credentials](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials)
    - [SCADA](https://github.com/scadastrangelove/SCADAPASS/tree/master)
- Create your own [[CBBH Modules (For Testing)/1. Recon/3. Brute Forcing/Wordlists]]
- Vulnerable Password Reset
    - Guessable Security Questions
    - Username Injection in Reset Request
## Authentication Bypasses
- Accessing the protected page directly
- Manipulating HTTP Parameters to access protected pages
## Session Attacks
- Brute-Forcing cookies with insufficient entropy
- Session Fixation
    - Attacker obtains valid session identifier
    - Attacker coerces victim to use this session identifier (social engineering)
    - Victim authenticates to the vulnerable web application
    - Attacker knows the victim's session identifier and can hijack their account
- Improper Session Timeout
    - Sessions should expire after an appropriate time interval
    - Session validity duration depends on the web application

