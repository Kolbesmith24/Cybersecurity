# PHP Filters
If we identify an LFI vulnerability in PHP web applications, then we can utilize different [PHP Wrappers](https://www.php.net/manual/en/wrappers.php.php) to be able to extend our LFI exploitation, and even potentially reach remote code execution.

PHP Wrappers allow us to access different I/O streams at the application level, like standard input/output, file descriptors, and memory streams.

We can utilize these wrappers to extend our exploitation attacks and be able to read PHP source code files or even execute system commands.
## Input Filters
[PHP Filters](https://www.php.net/manual/en/filters.php) are a type of PHP wrappers, where we can pass different types of input and have it filtered by the filter we specify. To use PHP wrapper streams, we can use the `php://` scheme in our string, and we can access the PHP filter wrapper with `php://filter/`.

The `filter` wrapper has several parameters, but the main ones we require for our attack are `resource` and `read`.
- `resource` parameter specifies the stream we would like to apply the filter on (e.g. a local file)
- `read` parameter applies different filters on the input resource, so we can use it to specify which filter we want to apply on our resource.

> There are four different types of filters available for use, which are [String Filters](https://www.php.net/manual/en/filters.string.php), [Conversion Filters](https://www.php.net/manual/en/filters.convert.php), [Compression Filters](https://www.php.net/manual/en/filters.compression.php), and [Encryption Filters](https://www.php.net/manual/en/filters.encryption.php).

The filter that is useful for LFI attacks is the `convert.base64-encode` filter, under `Conversion Filters`.
## Fuzzing for PHP Files
The first step would be to fuzz for different available PHP pages with a tool like `ffuf` or `gobuster`:
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php
```
> we should be scanning for all codes, including `301`, `302` and `403` pages, and we should be able to read their source code as well.

Even after reading the sources of any identified files, we can `scan them for other referenced PHP files`, and then read those as well, until we are able to capture most of the web application's source or have an accurate image of what it does.
## Standard PHP Inclusion
If you tried to include any php files through LFI, you would have noticed that the included PHP file gets executed, and eventually gets rendered as a normal HTML page.

This may be useful in certain cases, like accessing local PHP pages we do not have access over, but we would be more interested in reading the PHP source code through LFI. This is where the `base64` php filter gets useful, as we can use it to base64 encode the php file, and then we would get the encoded source code instead of having it being executed and rendered.
## Source Code Disclosure
Once we have a list of potential PHP files we want to read, we can start disclosing their sources with the `base64` PHP filter.

Let's try to read the source code of `config.php` using the base64 filter, by specifying `convert.base64-encode` for the `read` parameter and `config` for the `resource` parameter, as follows:
```url
php://filter/read=convert.base64-encode/resource=config
```
```
http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=config
```
> **Note:** We intentionally left the resource file at the end of our string, as the `.php` extension is automatically appended to the end of our input string, which would make the resource we specified be `config.php`.

using the base64 filter returned an encoded string instead of the empty result we saw earlier. We can now decode this string to get the content of the source code of `config.php`.