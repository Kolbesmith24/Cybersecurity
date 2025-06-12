[Response Headers](https://tools.ietf.org/html/rfc7231#section-7) can be `used in an HTTP response and do not relate to the content`. Certain response headers such as `Age`, `Location`, and `Server` are used to provide more context about the response. The following headers are commonly seen in HTTP responses.

|**Header**|**Example**|**Description**|
|---|---|---|
|`Server`|`Server: Apache/2.2.14 (Win32)`|Contains information about the HTTP server, which processed the request. It can be used to gain information about the server, such as its version, and enumerate it further.|
|`Set-Cookie`|`Set-Cookie: PHPSESSID=b4e4fbd93540`|Contains the cookies needed for client identification. Browsers parse the cookies and store them for future requests. This header follows the same format as the `Cookie` request header.|
|`WWW-Authenticate`|`WWW-Authenticate: BASIC realm="localhost"`|Notifies the client about the type of authentication required to access the requested resource.|