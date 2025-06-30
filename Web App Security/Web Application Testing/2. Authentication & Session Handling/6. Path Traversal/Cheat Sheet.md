# Quick Wins
1. Capture requests for resources when loading a page (such as images when clicking on a product)
	- Send to Burp
	- Try `../../../etc/passwd` on the request for the file
## Bypassing obstacles
1. Might be able to use an absolute path from the filesystem root, such as `filename=/etc/passwd`, to directly reference a file without using any traversal sequences.
2. You might be able to use nested traversal sequences, such as ``....//....//....//etc/passwd`` or `....\/....\/....\/etc/passwd`
3. In some contexts, such as in a URL path or the `filename` parameter of a `multipart/form-data` request, web servers may strip any directory traversal sequences before passing your input, you can sometimes bypass by URL encoding, or even double URL encoding, the `../` characters
	- results in `%2e%2e%2f` and `..%252f..%252f..%252fetc/passwd`
	- `..%c0%af` or `..%ef%bc%8f` may also work
4. An application may require the user-supplied filename to start with the expected base folder, such as `/var/www/images`
	- May have to start with `/var/www/images` and go from there with `../../etc/passwd`
5. May require a specific file extension at the end of the request such as `png`
	- May be able to bypass with a null byte: `filename=../../../etc/passwd%00.png`