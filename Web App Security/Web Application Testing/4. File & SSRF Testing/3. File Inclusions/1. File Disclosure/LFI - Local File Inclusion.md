# Local File Inclusion (LFI)
## Basic LFI: Example Walkthrough
In this exercise, we can set the language to English or Spanish.
- Once selected, we can see the URL includes the language:
```
http://<SERVER_IP>:<PORT>/index.php?language=es.php
```

So, if the web application is indeed pulling a file that is now being included in the page, we may be able to change the file being pulled to read the content of a different local file. 

Two common readable files that are available on most back-end servers are `/etc/passwd` on Linux and `C:\Windows\boot.ini` on Windows. So, let's change the parameter from `es` to `/etc/passwd`:
```
http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd
```
### Path Traversal
The previous example only works if the whole input was used within the `include()` function without any additions, like the following example:
```php
include($_GET['language']);
```

However, in many occasions, web developers may append or prepend a string to the `language` parameter. For example, the `language` parameter may be used for the filename, and may be added after a directory, as follows:
```php
include("./languages/" . $_GET['language']);
```

We can easily bypass this restriction by traversing directories using `relative paths`. To do so, we can add `../` before our file name, which refers to the parent directory.

So, we can use this trick to go back several directories until we reach the root path (i.e. `/`), and then specify our absolute file path (e.g. `../../../../etc/passwd`), and the file should exist:
```
http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/passwd
```
### Filename Prefix
On some occasions, our input may be appended after a different string. For example, it may be used with a prefix to get the full filename, like the following example:
```php
include("lang_" . $_GET['language']);
```

In this case, if we try to traverse the directory with `../../../etc/passwd`, the final string would be `lang_../../../etc/passwd`, which is invalid.

Instead of directly using path traversal, we can prefix a `/` before our payload, and this should consider the prefix as a directory, and then we should bypass the filename and be able to traverse directories:
```
http://<SERVER_IP>:<PORT>/index.php?language=/../../../etc/passwd
```
> **Note:** This may not always work, as in this example a directory named `lang_/` may not exist, so our relative path may not be correct.
### Appended Extensions
Another very common example is when an extension is appended to the `language` parameter, as follows:
```php
include($_GET['language'] . ".php");
```

In this case, if we try to read `/etc/passwd`, then the file included would be `/etc/passwd.php`, which does not exist.
- There are several techniques that we can use to bypass this, and we will discuss them in upcoming sections.
### Second-Order Attacks
Another common, and a little bit more advanced, LFI attack is a `Second Order Attack`. This occurs because many web application functionalities may be insecurely pulling files from the back-end server based on user-controlled parameters.

For example, a web application may allow us to download our avatar through a URL like (`/profile/$username/avatar.png`). If we craft a malicious LFI username (e.g. `../../../etc/passwd`), then it may be possible to change the file being pulled to another local file on the server and grab it instead of our avatar.

In this case, we would be poisoning a database entry with a malicious LFI payload in our username. Then, another web application functionality would utilize this poisoned entry to perform our attack (i.e. download our avatar based on username value). This is why this attack is called a `Second-Order` attack.