# Code Obfuscation
A technique used to make a script more difficult to read by humans but allows it to function the same from a technical point of view, though performance may be slower.

While `Python` and `PHP` usually reside on the server-side and hence are hidden from end-users, `JavaScript` is usually used within browsers at the `client-side`, and the code is sent to the user and executed in cleartext. This is why obfuscation is very often used with `JavaScript`.
### Use Cases
- To hide the original code and its functions to prevent it from being reused or copied without the developer's permission, making it more difficult to reverse engineer the code's original functionality
- To provide a security layer when dealing with authentication or encryption to prevent attacks on vulnerabilities that may be found within the code
- It is common for attackers and malicious actors to obfuscate their malicious scripts to prevent Intrusion Detection and Prevention systems from detecting their scripts
# Obfuscating Code
Take the following line of code as an example and attempt to obfuscate it:
```javascript
console.log('HTB JavaScript Deobfuscation Module');
```

We can go to [JSConsole](https://jsconsole.com), paste the code and hit enter, and see its output.
- this line of code prints `HTB JavaScript Deobfuscation Module`, which is done using the `console.log()` function.
### Minifying JavaScript code
Having the entire code in a single (often very long) line.

Many tools can help us minify JavaScript code, like [javascript-minifier](https://javascript-minifier.com/). We simply copy our code, and click `Minify`, and we get the minified output on the right.

Once again, we can copy the minified code to [JSConsole](https://jsconsole.com), and run it, and we see that it runs as expected. 

Usually, minified JavaScript code is saved with the extension `.min.js`.
### Packing JavaScript code
First, we will try [BeautifyTools](http://beautifytools.com/javascript-obfuscator.php) to obfuscate our code.

Paste our code in and click 'Obfuscate'.

We can copy this code into [https://jsconsole.com](https://jsconsole.com), to verify that it still does its main function.

A `packer` obfuscation tool usually attempts to convert all words and symbols of the code into a list or a dictionary and then refer to them using the `(p,a,c,k,e,d)` function to re-build the original code during execution.

While a packer does a great job reducing the code's readability, we can still see its main strings written in cleartext, which may reveal some of its functionality.
## Advanced Obfuscation
### Obfuscator
Let's visit [https://obfuscator.io](https://obfuscator.io). Before we click `obfuscate`, we will change `String Array Encoding` to `Base64`, as seen below:

![JavaScript obfuscation tool with options for compact code, string array manipulation, and sourcemaps.](https://academy.hackthebox.com/storage/modules/41/js_deobf_obfuscator_2.jpg)

Now, we can paste our code and click `obfuscate`:
![JavaScript code input area with 'Obfuscate' button.](https://academy.hackthebox.com/storage/modules/41/js_deobf_obfuscator_1.jpg)

We get the following code:
```javascript
var _0x1ec6=['Bg9N','sfrciePHDMfty3jPChqGrgvVyMz1C2nHDgLVBIbnB2r1Bgu='];(function(_0x13249d,_0x1ec6e5){var _0x14f83b=function(_0x3f720f){while(--_0x3f720f){_0x13249d['push'](_0x13249d['shift']());}};_0x14f83b(++_0x1ec6e5);}(_0x1ec6,0xb4));var _0x14f8=function(_0x13249d,_0x1ec6e5){_0x13249d=_0x13249d-0x0;var _0x14f83b=_0x1ec6[_0x13249d];if(_0x14f8['eOTqeL']===undefined){var _0x3f720f=function(_0x32fbfd){var _0x523045='abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789+/=',_0x4f8a49=String(_0x32fbfd)['replace'](/=+$/,'');var _0x1171d4='';for(var _0x44920a=0x0,_0x2a30c5,_0x443b2f,_0xcdf142=0x0;_0x443b2f=_0x4f8a49['charAt'](_0xcdf142++);~_0x443b2f&&(_0x2a30c5=_0x44920a%0x4?_0x2a30c5*0x40+_0x443b2f:_0x443b2f,_0x44920a++%0x4)?_0x1171d4+=String['fromCharCode'](0xff&_0x2a30c5>>(-0x2*_0x44920a&0x6)):0x0){_0x443b2f=_0x523045['indexOf'](_0x443b2f);}return _0x1171d4;};_0x14f8['oZlYBE']=function(_0x8f2071){var _0x49af5e=_0x3f720f(_0x8f2071);var _0x52e65f=[];for(var _0x1ed1cf=0x0,_0x79942e=_0x49af5e['length'];_0x1ed1cf<_0x79942e;_0x1ed1cf++){_0x52e65f+='%'+('00'+_0x49af5e['charCodeAt'](_0x1ed1cf)['toString'](0x10))['slice'](-0x2);}return decodeURIComponent(_0x52e65f);},_0x14f8['qHtbNC']={},_0x14f8['eOTqeL']=!![];}var _0x20247c=_0x14f8['qHtbNC'][_0x13249d];return _0x20247c===undefined?(_0x14f83b=_0x14f8['oZlYBE'](_0x14f83b),_0x14f8['qHtbNC'][_0x13249d]=_0x14f83b):_0x14f83b=_0x20247c,_0x14f83b;};console[_0x14f8('0x0')](_0x14f8('0x1'));
```

We can now try running it in [https://jsconsole.com](https://jsconsole.com) to verify that it still performs its original function.
### More Obfuscation
We can try obfuscating code using the same tool in [JSF](http://www.jsfuck.com), and then rerunning it. We will notice that the code may take some time to run, which shows how code obfuscation could affect the performance.

There are many other JavaScript obfuscators, like [JJ Encode](https://utf-8.jp/public/jjencode.html) or [AA Encode](https://utf-8.jp/public/aaencode.html). However, such obfuscators usually make code execution/compilation very slow, so it is not recommended to be used unless for an obvious reason, like bypassing web filters or restrictions.
## Deobfuscation
### Beautify
In order to properly format `Minified JavaScript` code, we need to `Beautify` our code. The most basic method for doing so is through our `Browser Dev Tools`.

if we were using Firefox, we can open the browser debugger with [ `CTRL+SHIFT+Z` ], and then click on our script `secret.js`. We can click on the '`{ }`' button at the bottom, which will `Pretty Print` the script into its proper JavaScript formatting.

Furthermore, we can utilize many online tools or code editor plugins, like [Prettier](https://prettier.io/playground/) or [Beautifier](https://beautifier.io/).
### Deobfuscate
[UnPacker](https://matthewfl.com/unPacker.html) is a good deobfuscate tool for JavaScript
### Reverse Engineering
Sometimes the code is more obfuscated and encoding, which means we would need to manually reverse engineer the code to understand how it was obfuscated and its functionality for such cases.

check out the [Secure Coding 101](https://academy.hackthebox.com/module/details/38) module, which should thoroughly cover this topic.
Here's a **summary of the key points** from the provided text on decoding and encoding techniques:
#### 1. Base64 Encoding
- **Used to reduce special characters**; output is alphanumeric with `+`, `/`, and padded with =.
- **Spotting it**: Recognizable due to its character set and = padding at the end.
**Encode in Linux**:
```bash
echo 'text' | base64
```

**Decode in Linux**:
```bash
echo 'encoded_string' | base64 -d
```

---
#### Hex Encoding
- Encodes characters as their **hex ASCII values**.
- **Spotting it**: Only uses digits `0-9` and letters `a-f`.
**Encode in Linux**:
```bash
echo 'text' | xxd -p
```

**Decode in Linux**:
```bash
echo 'hex_string' | xxd -p -r
```

---
#### Caesar Cipher / ROT13
- A simple **letter-shifting cipher**.
- **ROT13** is a specific case that shifts letters by 13 positions.
- **Spotting it**: Results may look jumbled but retain structural hints (e.g., `http` becomes `uggc`).

**Encode/Decode in Linux**:
```bash
echo 'text' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

---
#### Other Encodings & Tools
- Many other encoding formats exist; some require experience to recognize.
- **Tools like [Cipher Identifier](https://www.boxentriq.com/code-breaking/cipher-identifier)** can help detect encoding types automatically.
- Be aware of **encryption**, which is different from encoding and requires a **key** to decrypt.
