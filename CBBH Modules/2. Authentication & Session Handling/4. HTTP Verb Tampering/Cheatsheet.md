# HTTP Verb Tampering
## HTTP Method
- `HEAD`
- `PUT`
- `DELETE`
- `OPTIONS`
- `PATCH`

|**Command**|**Description**|
|---|---|
|`-X OPTIONS`|Set HTTP Method with Curl|
## When to check for this
- When receiving the following errors when attempting to visit directories/endpoints
	- 405
	- 401 
	- Not Authorized
- Trying to upload a malicious file that is being denied