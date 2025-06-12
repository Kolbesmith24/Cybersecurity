# Command Injection Prevention
## System Commands
We should always avoid using functions that execute system commands, especially if we are using user input with them.

Instead of using system command execution functions, we should use built-in functions that perform the needed functionality, as back-end languages usually have secure implementations of these types of functionalities. For example, suppose we wanted to test whether a particular host is alive with `PHP`. In that case, we may use the `fsockopen` function instead, which should not be exploitable to execute arbitrary system commands.
## Input Validation
We should always validate and then sanitize the user input. Input validation is done to ensure it matches the expected format for the input, such that the request is denied if it does not match. In our example web application, we saw that there was an attempt at input validation on the front-end, but `input validation should be done both on the front-end and on the back-end`.

In `PHP`, like many other web development languages, there are built in filters for a variety of standard formats, like emails, URLs, and even IPs, which can be used with the `filter_var` function.

If we wanted to validate a different non-standard format, then we can use a Regular Expression `regex` with the `preg_match` function. The same can be achieved with `JavaScript` for both the front-end and back-end (i.e. `NodeJS`).

Just like `PHP`, with `NodeJS`, we can also use libraries to validate various standard formats, like [is-ip](https://www.npmjs.com/package/is-ip) for example, which we can install with `npm`, and then use the `isIp(ip)` function in our code.
## Input Sanitization
The most critical part for preventing any injection vulnerability is input sanitization, which means removing any non-necessary special characters from the user input.

We can use `preg_replace` to remove any special characters from the user input.

The same can be done with `JavaScript`, as follows:
```javascript
var ip = ip.replace(/[^A-Za-z0-9.]/g, '');
```

We can also use the DOMPurify library for a `NodeJS` back-end, as follows:
```javascript
import DOMPurify from 'dompurify';
var ip = DOMPurify.sanitize(ip);
```

In certain cases, we may want to allow all special characters (e.g., user comments), then we can use the same `filter_var` function we used with input validation, and use the `escapeshellcmd` filter to escape any special characters, so they cannot cause any injections. For `NodeJS`, we can simply use the `escape(ip)` function.
## Server Configuration
Finally, we should make sure that our back-end server is securely configured to reduce the impact in the event that the webserver is compromised. Some of the configurations we may implement are:
- Use the web server's built-in Web Application Firewall (e.g., in Apache `mod_security`), in addition to an external WAF (e.g. `Cloudflare`, `Fortinet`, `Imperva`..)
- Abide by the [Principle of Least Privilege (PoLP)](https://en.wikipedia.org/wiki/Principle_of_least_privilege) by running the web server as a low privileged user (e.g. `www-data`)
- Prevent certain functions from being executed by the web server (e.g., in PHP `disable_functions=system,...`)
- Limit the scope accessible by the web application to its folder (e.g. in PHP `open_basedir = '/var/www/html'`)
- Reject double-encoded requests and non-ASCII characters in URLs
- Avoid the use of sensitive/outdated libraries and modules (e.g. [PHP CGI](https://www.php.net/manual/en/install.unix.commandline.php))