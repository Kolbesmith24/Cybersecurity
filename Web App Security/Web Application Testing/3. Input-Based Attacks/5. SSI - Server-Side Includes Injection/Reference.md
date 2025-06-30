# Server-Side Includes (SSI) Injection
Server-side includes (SSI) can be used to generate HTML responses dynamically. SSI directives instruct the webserver to include additional content dynamically. These directives are embedded into HTML files. For instance, SSI can be used to include content that is present in all HTML pages, such as headers or footers. 

When an attacker can inject commands into the SSI directives, [Server-Side Includes (SSI) Injection](https://owasp.org/www-community/attacks/Server-Side_Includes_\(SSI\)_Injection) can occur. 

SSI injection can lead to data leakage or even remote code execution.

The use of SSI can often be inferred from the file extension. Typical file extensions include `.shtml`, `.shtm`, and `.stm`. However, web servers can be configured to support SSI directives in arbitrary file extensions. As such, we cannot conclusively conclude whether SSI is used only from the file extension.
## SSI Directives
SSI utilizes `directives` to add dynamically generated content to a static HTML page. These directives consist of the following components:
- `name`: the directive's name
- `parameter name`: one or more parameters
- `value`: one or more parameter values

An SSI directive has the following syntax:
```ssi
<!--#name param1="value1" param2="value" -->
```

For instance, the following are some common SSI directives.
#### printenv
This directive prints environment variables. It does not take any variables.
```ssi
<!--#printenv -->
```
#### config
This directive changes the SSI configuration by specifying corresponding parameters. For instance, it can be used to change the error message using the `errmsg` parameter:
```ssi
<!--#config errmsg="Error!" -->
```
#### echo
Prints the value of any cariable given in the `var` parameter:
```ssi
<!--#echo var="DOCUMENT_NAME" var="DATE_LOCAL" -->
```
#### exec
This directive executes the command given in the `cmd` parameter:
```ssi
<!--#exec cmd="whoami" -->
```
#### include
This directive includes the file specified in the `virtual` parameter. It only allows for the inclusion of files in the web root directory.
```ssi
<!--#include virtual="index.html" -->
```
## Exploiting SSI Injection
If we see a page URL that has the extension for SSI (such as `.shtml`), we can start to try to exploit it.
- These are usually pages where user-input is supplied (such as entering your name and it displaying your name)

We can test for SSI Injection with the following input instead:
```ssi
<!--#printenv -->
```

If the directive is executed, we can try to execute arbitrary commands using the `exec` directive by providing the following username: 
```ssi
<!--#exec cmd="id" -->
```