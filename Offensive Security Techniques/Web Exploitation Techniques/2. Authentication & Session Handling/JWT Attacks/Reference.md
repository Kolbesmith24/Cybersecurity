Note: Install `JWT Editor` extension on Burp Suite
# JWTs
Flawed handling of JSON web tokens (JWTs) can leave websites vulnerable to a variety of high-severity attacks.

JWTs are most commonly used in authentication, session management, and access control mechanisms, so these vulnerabilities can potentially compromise the entire website and its users.
![[jwt-infographic.jpg]]

## What are JWTs?
JSON web tokens (JWTs) are a standardized format for sending cryptographically signed JSON data between systems. They can theoretically contain any kind of data, but are most commonly used to send information ("claims") about users as part of authentication, session handling, and access control mechanisms.

Unlike with classic session tokens, all of the data that a server needs is stored client-side within the JWT itself. This makes JWTs a popular choice for highly distributed websites where users need to interact seamlessly with multiple back-end servers.
### JWT Format
A JWT consists of 3 parts: 
1. Header
2. Payload
3. Signature

These are each separated by a dot, as shown in the following example:
```
eyJraWQiOiI5MTM2ZGRiMy1jYjBhLTRhMTktYTA3ZS1lYWRmNWE0NGM4YjUiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTY0ODAzNzE2NCwibmFtZSI6IkNhcmxvcyBNb250b3lhIiwic3ViIjoiY2FybG9zIiwicm9sZSI6ImJsb2dfYXV0aG9yIiwiZW1haWwiOiJjYXJsb3NAY2FybG9zLW1vbnRveWEubmV0IiwiaWF0IjoxNTE2MjM5MDIyfQ.SYZBPIBg2CRjXAJ8vCER0LA_ENjII1JakvNQoP-Hw6GG1zfl4JyngsZReIfqRvIAEi5L4HV0q7_9qGhQZvy9ZdxEJbwTxRs_6Lb-fZTDpW6lKYNdMyjw45_alSCZ1fypsMWz_2mTpQzil0lOtps5Ei_z7mM7M8gCwe_AGpI53JxduQOaB5HkT5gVrv9cKu9CsW5MS6ZbqYXpGyOG5ehoxqm8DL5tFYaW3lB50ELxi0KsuTKEbD0t5BCl0aCR2MBJWAbN-xeLwEenaqBiwPVvKixYleeDQiBEIylFdNNIMviKRgXiYuAvMziVPbwSgkZVHeEdF5MQP1Oe2Spac-6IfA
```

The header and payload parts of a JWT are just base64url-encoded JSON objects.
- The **header contains metadata about the token itself**, while **the payload contains the actual "claims" about the user.**

For example, you can decode the payload from the token above to reveal the following claims:
```
{
  "iss": "portswigger", 
  "exp": 1648037164, 
  "name": "Carlos Montoya", 
  "sub": "carlos", 
  "role": "blog_author", 
  "email": "carlos@carlos-montoya.net", 
  "iat": 1516239022 
}
```
- In most cases, this data can be easily read or modified by anyone with access to the token. Therefore, the security of any JWT-based mechanism is heavily reliant on the cryptographic signature.
### JWT Signature
The server that issues the token typically generates the signature by hashing the header and payload (sometimes also encrypting the hash).
- This process involves a secret signing key.

This mechanism provides a way for servers to verify that none of the data within the token has been tampered with since it was issued:
- As the signature is directly derived from the rest of the token, changing a single byte of the header or payload results in a mismatched signature.
- Without knowing the server's secret signing key, it shouldn't be possible to generate the correct signature for a given header or payload.
### JWT vs JWS vs JWE
The JWT specification is very limited. It only defines a format for representing information ("claims") as a JSON object that can be transferred between two parties.

In practice, JWTs aren't really used as a standalone entity. The JWT spec is extended by both the JSON Web Signature (JWS) and JSON Web Encryption (JWE) specifications, which define concrete ways of actually implementing JWTs.

![[jwt-jws-jwe.jpg]]

In other words, a JWT is usually either a JWS or JWE token. When people use the term "JWT", they almost always mean a JWS token. JWEs are very similar, except that the actual contents of the token are encrypted rather than just encoded.
- "JWT" refers primarily to JWS tokens, although some of the vulnerabilities described may also apply to JWE tokens.
## What are JWT attacks?
Involve a user sending modified JWTs to the server in order to achieve a malicious goal. 

Typically, this goal is to bypass authentication and access controls by impersonating another user who has already been authenticated.
## Impact of JWT Attacks
Usually severe. If an attacker is able to create their own valid tokens with arbitrary values, they may be able to escalate their own privileges or impersonate other users, taking full control of their accounts.
## How do vulnerabilities to JWT attacks arise?
Typically arise due to flawed JWT handling within the application itself.
- JWTs have various options for web developers to decide to implement, which can result in them accidentally introducing vulnerabilities

These implementation flaws usually mean that the signature of the JWT is not verified properly. This enables an attacker to tamper with the values passed to the application via the token's payload. 

Even if the signature is robustly verified, whether it can truly be trusted relies heavily on the server's secret key remaining a secret. If this key is leaked in some way, or can be guessed or brute-forced, an attacker can generate a valid signature for any arbitrary token, compromising the entire mechanism.
## Using Burp Suite for JWT Tokens
Using `HTTP history` in the `Proxy` tab, select the highlighted request, highlight the section of the JWT token in the request, and view the contents in the `Inspector` section all the way to the right:
![[Pasted image 20250508074707.png]]

To edit, right click the highlighted text, send to `Repeater`, then go to the `JSON Web Token` tab in the request panel, edit the content in the Header / Payload section, then select `Sign` at the bottom:
![[Pasted image 20250508074924.png]]

A dialogue will open up. Select the appropriate signing key and select `OK`. You can now send your request, select `Follow Redirection` at the top, and observe the reaction.

If you want to create a new key, go to the `JWT Editor Keys` in the top right tab, select `New Symmetric Key`, then Generate your key.
## Exploiting flawed JWT signature verification
Servers don't usually store any information about the JWTs that they issue. Instead, each token is an entirely self-contained entity.

This has several advantages, but also introduces a fundamental problem - the server doesn't actually know anything about the original contents of the token, or even what the original signature was. Therefore, if the server doesn't verify the signature properly, there's nothing to stop an attacker from making arbitrary changes to the rest of the token.

For example, consider a JWT containing the following claims:
```
{ 
  "username": "carlos", 
  "isAdmin": false 
}
```

If the server identifies the session based on this `username`, modifying its value might enable an attacker to impersonate other logged-in users. Similarly, if the `isAdmin` value is used for access control, this could provide a simple vector for privilege escalation.
### Accepting arbitrary signatures
JWT libraries typically provide one method for verifying tokens and another that just decodes them.
- For example, the Node.js library `jsonwebtoken` has `verify()` and `decode()`.

Occasionally, developers confuse these two methods and only pass incoming tokens to the `decode()` method. This effectively means that the application doesn't verify the signature at all.
### Accepting tokens with no signature
# Maintaining an Authenticated Session
When testing, some actions may result in an application terminating your session. For example, an application may automatically log you out if you submit suspicious input.

Burp enables you to configure a session handling rule to automatically log back into an application. The session handling rule determines whether a session is valid. If it's invalid, it will run a macro to update the session cookies and log back in.

The process consists of three steps:
1. Identifying a valid login expression.
2. Configuring a session handling rule.
3. Checking the session handling rule.
## Identifying an invalid login expression
Before you configure a session handling rule, you need to identify how the target site behaves when the session is invalid.
1. In Burp's browser, log in to the target site using valid credentials.
2. Go to a page that requires you to be logged in to access it.
3. Log out.
4. Try to get back to **My Account** without logging in.
5. In Burp, go to the **Proxy > HTTP history** tab to identify the behavior of the target site when an unauthorized user tries to access a restricted page.
## Configuring a session handling rule
Enables you to maintain an authenticated session:
1. Click **Settings** to open the **Settings** dialog.
2. Under **Sessions > Session handling rules**, click **Add**. The session handling rule editor opens.
3. Go to the **Scope** tab. Select the tools and URLs that you want the rule to apply to. The default tool scope and the suite URL scope are suitable for most use cases.
4. Go to the **Details** tab. Add a unique rule description.
5. Under **Rule actions**, click **Add**, then select **Check session is valid** from the drop-down menu. The session handling action editor opens.
![[active-session-1.png]]
6. Under **Inspect response to determine session validity**, specify the expression that is found in an invalid login response. This should be the expression you identified earlier. Also, specify the aspects of each in-scope response that Burp should inspect for the expression:
    - **Location(s)** - Select the locations in the response that you want Burp to inspect. If you're using `ginandjuice.shop`, select **URL of redirection target**.
    - **Look for expression** - Specify the expression that is found in a valid login response. If you're using `ginandjuice.shop`, enter `login`.
    - **Match type** - Select whether the expression is a literal string or regex. If you're using `ginandjuice.shop`, select **Literal string**.
    - **Case-sensitivity** - Select whether the expression is case-sensitive or insensitive. If you're using `ginandjuice.shop`, select **Insensitive**.
    - **Match indicates** - Select whether a match indicates that the session is valid or invalid. If you're using `ginandjuice.shop`, select **Invalid session**.
7. Under **Define behavior dependent on session validity**, select **If session is invalid, perform the action below > Run a macro**.
8. Click **Add**. The **Macro editor** and **Macro recorder** dialogs open.
9. In the **Macro recorder** dialog, select the login requests, then click **OK**. If you're using `ginandjuice.shop`, select the `GET /login` and the two `POST /login` requests.
 ![[active-session-2.png]]
10. Click **OK** to close all open dialogs. The rule is added to the list of session handling rules.
## Checking the session handling rule
It's a good idea to check that the session handling rule works. To do this:

1. In Burp's browser, log out of the website.
2. In **Proxy > HTTP history**, identify a request for a page that you need to be logged in to access. For example, if you're using `ginandjuice.shop`, you can use a `GET /my-account` request. The page should contain a session cookie that is now invalid.
3. Right-click the request and select **Send to Repeater**.
4. Go to the **Repeater** tab and send the request. Notice that the session cookies automatically update.
5. Review the response to confirm that you've logged in successfully.
![[active-session-3.png]]