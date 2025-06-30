HTTP Security headers are `a class of response headers used to specify certain rules and policies` to be followed by the browser while accessing the website.

|**Header**|**Example**|**Description**|
|---|---|---|
|`Content-Security-Policy`|`Content-Security-Policy: script-src 'self'`|Dictates the website's policy towards externally injected resources. This could be JavaScript code as well as script resources. This header instructs the browser to accept resources only from certain trusted domains, hence preventing attacks such as [Cross-site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting).|
|`Strict-Transport-Security`|`Strict-Transport-Security: max-age=31536000`|Prevents the browser from accessing the website over the plaintext HTTP protocol, and forces all communication to be carried over the secure HTTPS protocol. This prevents attackers from sniffing web traffic and accessing protected information such as passwords or other sensitive data.|
|`Referrer-Policy`|`Referrer-Policy: origin`|Dictates whether the browser should include the value specified via the `Referer` header or not. It can help in avoiding disclosing sensitive URLs and information while browsing the website.|
A complete list of standard HTTP headers can be found [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).