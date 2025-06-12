# Client-Side Validation
Many web applications only rely on front-end JavaScript code to validate the selected file format before it is uploaded and would not upload it if the file is not in the required format (e.g., not an image).

We can easily bypass it by directly interacting with the server, skipping the front-end validations altogether. We may also modify the front-end code through our browser's dev tools to disable any validation in place.
## Client-Side Validation
This time, when we get the file selection dialog, we cannot see our `PHP` scripts (or it may be greyed out), as the dialog appears to be limited to image formats only.

We may still select the `All Files` option to select our `PHP` script anyway, but when we do so, we get an error message saying (`Only images are allowed!`), and the `Upload` button gets disabled.

This indicates some form of file type validation, so we cannot just upload a web shell through the upload form as we did in the previous section. 

Luckily, all validation appears to be happening on the front-end, as the page never refreshes or sends any HTTP requests after selecting our file. So, we should be able to have complete control over these client-side validations.

To bypass these protections, we can either `modify the upload request to the back-end server`, or we can `manipulate the front-end code to disable these type validations`.
### Back-end Request Modification
If we capture the upload request with `Burp`, we can see the request data at the bottom stating it is a PNG and the data in red:
![HTTP POST request to /upload.php with file HTB.png, content type image/png.](https://academy.hackthebox.com/storage/modules/136/file_uploads_image_upload_request.jpg)

The two important parts in the request are `filename="HTB.png"` and the file content at the end of the request. 

If we modify the `filename` to `shell.php` and modify the content to the web shell we used in the previous section; we would be uploading a `PHP` web shell instead of an image:
![HTTP POST request to /upload.php with file shell.php, response 200 OK, file successfully uploaded.](https://academy.hackthebox.com/storage/modules/136/file_uploads_modified_upload_request.jpg)
### Disabling Front-end Validation
Another method to bypass client-side validations is through manipulating the front-end code. As these functions are being completely processed within our web browser, we have complete control over them. So, we can modify these scripts or disable them entirely. Then, we may use the upload functionality to upload any file type without needing to utilize `Burp` to capture and modify our requests.

To start, we can click [`CTRL+SHIFT+C`] to toggle the browser's `Page Inspector`, and then click on the profile image, which is where we trigger the file selector for the upload form.

We can find the HTML line in inspector and modify it to `All Files`. The more interesting part is `onchange="checkFile(this)"`, which appears to run a JavaScript code whenever we select a file, which appears to be doing the file type validation. To get the details of this function, we can go to the browser's `Console` by clicking [`CTRL+SHIFT+K`], and then we can type the function's name (`checkFile`) to get its details:

We can remove this function from the HTML code since its primary use appears to be file type validation, and removing it should not break anything.
- To do so, we can go back to our inspector, click on the profile image again, double-click on the function name (`checkFile`) on line `18`, and delete it.
- **Tip:** You may also do the same to remove `accept=".jpg,.jpeg,.png"`
- With the `checkFile` function removed from the file input, we should be able to select our `PHP` web shell through the file selection dialog and upload it normally with no validations
> **Note:** The modification we made to the source code is temporary and will not persist through page refreshes

Once we upload our web shell using either of the above methods and then refresh the page, we can use the `Page Inspector` once more with [`CTRL+SHIFT+C`], click on the profile image, and we should see the URL of our uploaded web shell.
# Blacklist Filters
## Blacklisting Extensions
There are generally two common forms of validating a file extension on the back-end:

1. Testing against a `blacklist` of types
2. Testing against a `whitelist` of types

Furthermore, the validation may also check the `file type` or the `file content` for type matching. The weakest form of validation amongst these is `testing the file extension against a blacklist of extension`

The code is taking the file extension (`$extension`) from the uploaded file name (`$fileName`) and then comparing it against a list of blacklisted extensions (`$blacklist`). However, this validation method has a major flaw. `It is not comprehensive`, as many other extensions are not included in this list, which may still be used to execute PHP code on the back-end server if uploaded.

> **Tip:** The comparison above is also case-sensitive, and is only considering lowercase extensions. In Windows Servers, file names are case insensitive, so we may try uploading a `php` with a mixed-case (e.g. `pHp`), which may bypass the blacklist as well, and should still execute as a PHP script.
## Fuzzing Extensions
`PayloadsAllTheThings` provides lists of extensions for [PHP](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst) and [.NET](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Extension%20ASP) web applications. We may also use `SecLists` list of common [Web Extensions](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt).

From `Burp History`, we can locate our last request to `/upload.php`, right-click on it, and select `Send to Intruder`. From the `Positions` tab, we can `Clear` any automatically set positions, and then select the `.php` extension in `filename="HTB.php"` and click the `Add` button to add it as a fuzzing position.

Finally, we can `Load` the PHP extensions list from above in the `Payloads` tab under `Payload Options`. We will also un-tick the `URL Encoding` option to avoid encoding the (`.`) before the file extension. Once this is done, we can click on `Start Attack` to start fuzzing for file extensions that are not blacklisted.
## Non-Blacklisted Extensions
Now, we can try uploading a file using any of the `allowed extensions` from above, and some of them may allow us to execute PHP code. `Not all extensions will work with all web server configurations`, so we may need to try several extensions to get one that successfully executes PHP code.
# Whitelist Filters
Run the same fuzzing test to try to find any extensions that we may be able to use.

If nothing is available, you can try a few things such as reading the source code and looking for vulnerable code, or double extensions.

One common example is using regex, which only checks whether the file name `contains` the extension and not if it actually `ends` with it. Many developers make such mistakes due to a weak understanding of regex patterns.
## Double Extensions
A straightforward method of passing the regex test is through `Double Extensions`. For example, if the `.jpg` extension was allowed, we can add it in our uploaded file name and still end our filename with `.php` (e.g. `shell.jpg.php`).

However, this may not always work, as some web applications may use a strict `regex` pattern, as mentioned earlier, like the following:
```php
if (!preg_match('/^.*\.(jpg|jpeg|png|gif)$/', $fileName)) { ...SNIP... }
```
This pattern should only consider the final file extension, as it uses (`^.*\.`) to match everything up to the last (`.`), and then uses (`$`) at the end to only match extensions that end the file name. So, the `above attack would not work`.
- Nevertheless, some exploitation techniques may allow us to bypass this pattern, but most rely on misconfigurations or outdated systems.
## Reverse Double Extension
In some cases, the file upload functionality itself may not be vulnerable, but the web server configuration may lead to a vulnerability. For example, an organization may use an open-source web application, which has a file upload functionality.

For example, the `/etc/apache2/mods-enabled/php7.4.conf` for the `Apache2` web server may include the following configuration:
```xml
<FilesMatch ".+\.ph(ar|p|tml)">
    SetHandler application/x-httpd-php
</FilesMatch>
```
The above configuration is how the web server determines which files to allow PHP code execution. It specifies a whitelist with a regex pattern that matches `.phar`, `.php`, and `.phtml`. However, this regex pattern can have the same mistake we saw earlier if we forget to end it with (`$`). In such cases, any file that contains the above extensions will be allowed PHP code execution, even if it does not end with the PHP extension.
- For example, the file name (`shell.php.jpg`) should pass the earlier whitelist test as it ends with (`.jpg`), and it would be able to execute PHP code due to the above misconfiguration, as it contains (`.php`) in its name.
## Character Injection
We can inject several characters before or after the final extension to cause the web application to misinterpret the filename and execute the uploaded file as a PHP script.

The following are some of the characters we may try injecting:
- `%20`
- `%0a`
- `%00`
- `%0d0a`
- `/`
- `.\`
- `.`
- `…`
- `:`


Each character has a specific use case that may trick the web application to misinterpret the file extension. For example, (`shell.php%00.jpg`) works with PHP servers with version `5.X` or earlier, as it causes the PHP web server to end the file name after the (`%00`), and store it as (`shell.php`), while still passing the whitelist.
> The same may be used with web applications hosted on a Windows server by injecting a colon (`:`) before the allowed file extension (e.g. `shell.aspx:.jpg`), which should also write the file as (`shell.aspx`).

We can write a small bash script that generates all permutations of the file name, where the above characters would be injected before and after both the `PHP` and `JPG` extensions, as follows:
```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
    for ext in '.php' '.phps'; do
        echo "shell$char$ext.jpg" >> wordlist.txt
        echo "shell$ext$char.jpg" >> wordlist.txt
        echo "shell.jpg$char$ext" >> wordlist.txt
        echo "shell.jpg$ext$char" >> wordlist.txt
    done
done
```

With this custom wordlist, we can run a fuzzing scan with `Burp Intruder`, similar to the ones we did earlier. If either the back-end or the web server is outdated or has certain misconfigurations, some of the generated filenames may bypass the whitelist test and execute PHP code.
## Type Filters
Furthermore, we may utilize some allowed extensions (e.g., SVG) to perform other attacks. All of this indicates that only testing the file extension is not enough to prevent file upload attacks.

Content filters usually specify a single category (e.g., images, videos, documents), which is why they do not typically use blacklists or whitelists. This is because web servers provide functions to check for the file content type, and it usually falls under a specific category.

There are two common methods for validating the file content: `Content-Type Header` or `File Content`.
### Content-Type
As the file extension does not affect the error message, the web application must be testing the file content for type validation. As mentioned earlier, this can be either in the `Content-Type Header` or the `File Content`.

The following is an example of how a PHP web application tests the Content-Type header to validate the file type:
```php
$type = $_FILES['uploadFile']['type'];

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```

The code sets the (`$type`) variable from the uploaded file's `Content-Type` header. Our browsers automatically set the Content-Type header when selecting a file through the file selector dialog, usually derived from the file extension. However, since our browsers set this, this operation is a client-side operation, and we can manipulate it to change the perceived file type and potentially bypass the type filter.

We may start by fuzzing the Content-Type header with SecLists' [Content-Type Wordlist](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-all-content-types.txt) through Burp Intruder, to see which types are allowed. However, the message tells us that only images are allowed, so we can limit our scan to image types, which reduces the wordlist to `45` types only (compared to around 700 originally). We can do so as follows:
```bash
wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/Web-Content/web-all-content-types.txt

cat web-all-content-types.txt | grep 'image/' > image-content-types.txt
```

We can then take this list and put it into our Burp Intruder payload, with the position for the payloads to target the data of `Content-Type`:
![[Pasted image 20250602173329.png]]
- We can then check what extensions are allowed that we may be able to exploit
	- Ex: svg, php, etc...

If SVG are allowed, and the image is reflected to us, we can refer to the [[Miscellaneous Upload Attacks#XXE]] payloads that use SVG to read the source code.

We can create a file with this payload and upload it. We can change the name of the file to `.jpeg` upload and capture it in Burp, switch it in Burp alongside with the `Content-Type` and `filename` data, and send.

> **Note:** A file upload HTTP request has two Content-Type headers, one for the attached file (at the bottom), and one for the full request (at the top). We usually need to modify the file's Content-Type header, but in some cases the request will only contain the main Content-Type header (e.g. if the uploaded content was sent as `POST` data), in which case we will need to modify the main Content-Type header.
## MIME-Type
The second and more common type of file content validation is testing the uploaded file's `MIME-Type`. `Multipurpose Internet Mail Extensions (MIME)` is an internet standard that determines the type of a file through its general format and bytes structure.

This is usually done by inspecting the first few bytes of the file's content, which contain the [File Signature](https://en.wikipedia.org/wiki/List_of_file_signatures) or [Magic Bytes](https://web.archive.org/web/20240522030920/https://opensource.apple.com/source/file/file-23/file/magic/magic.mime). For example, if a file starts with (`GIF87a` or `GIF89a`), this indicates that it is a `GIF` image, while a file starting with plaintext is usually considered a `Text` file.
- If we change the first bytes of any file to the GIF magic bytes, its MIME type would be changed to a GIF image, regardless of its remaining content or extension.
> **Tip:** Many other image types have non-printable bytes for their file signatures, while a `GIF` image starts with ASCII printable bytes (as shown above), so it is the easiest to imitate. Furthermore, as the string `GIF8` is common between both GIF signatures, it is usually enough to imitate a GIF image.
### Example
Let's take a basic example to demonstrate this. The `file` command on Unix systems finds the file type through the MIME type. If we create a basic file with text in it, it would be considered as a text file, as follows:
```bash
echo "this is a text file" > text.jpg 
file text.jpg 
```

As we see, the file's MIME type is `ASCII text`, even though its extension is `.jpg`. However, if we write `GIF8` to the beginning of the file, it will be considered as a `GIF` image instead:
```bash
echo "GIF8" > text.jpg 
file text.jpg
```

Web servers can also utilize this standard to determine file types, which is usually more accurate than testing the file extension. The following example shows how a PHP web application can test the MIME type of an uploaded file:
```php
$type = mime_content_type($_FILES['uploadFile']['tmp_name']);

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```

As we can see, the MIME types are similar to the ones found in the Content-Type headers, but their source is different, as PHP uses the `mime_content_type()` function to get a file's MIME type. Let's try to repeat our last attack, but now with an exercise that tests both the Content-Type header and the MIME type:
![POST request to /upload.php with file 'shell.php' disguised as image.](https://academy.hackthebox.com/storage/modules/136/file_uploads_bypass_content_type_request.jpg)

Once we forward our request, we notice that we get the error message `Only images are allowed`. Now, let's try to add `GIF8` before our PHP code to try to imitate a GIF image while keeping our file extension as `.php`, so it would execute PHP code regardless:
![POST request to /upload.php with file 'shell.php' disguised as image.](https://academy.hackthebox.com/storage/modules/136/file_uploads_bypass_mime_type_request.jpg)

>**Note:** When we go to the file on our web browser, we see that the command output starts with `GIF8` , as this was the first line in our PHP script to imitate the GIF magic bytes
## Using Multiple Methods

We can use a combination of the two methods discussed in this section, which may help us bypass some more robust content filters. For example, we can try using an `Allowed MIME type with a disallowed Content-Type`, an `Allowed MIME/Content-Type with a disallowed extension`, or a `Disallowed MIME/Content-Type with an allowed extension`, and so on. 

Similarly, we can attempt other combinations and permutations to try to confuse the web server, and depending on the level of code security, we may be able to bypass various filters.
## HTB Question Examples:
1. Add the `GIF8` to the first line to be read as a gif
2. Add the PHP payload on a new line
3. Test to make sure the request still sends (with allowed extension for the filename)
4. Try to bypass the whitelist and blacklist filters by testing the extension of the `filename` in the Intruder tab with https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files
5. Disable URL encoding, run the attack, and find any anomalies (such as a different error code)
6. Conform the response to the filter technique (Only images allowed = double extension method)
## Fuzzing Content-Type
Because any file with an extension not ending with that of an image can't be uploaded, the best attempt students can take is to name a shell file as `shell.phar.jpg`. 

However, this file can only be uploaded if the `Content-Type` header of the original image is not modified. 

Therefore, we need to fuzz the `Content-Type` header value. 

First, we need to add a payload marker around the value of `Content-Type`, such that it becomes `§image/jpeg§`. Then download [web-all-content-types.txt](https://github.com/danielmiessler/SecLists/raw/master/Discovery/Web-Content/web-all-content-types.txt):
```shell
wget https://github.com/danielmiessler/SecLists/raw/master/Discovery/Web-Content/web-all-content-types.txt
```

Then copy and paste all of these into Burp Intruder and fuzz the Content-Type. (MAKE SURE YOU UN-CHECK URL ENCODE)