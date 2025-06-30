# Types of XSS
There are three main types of XSS vulnerabilities:

|Type|Description|
|---|---|
|`Stored (Persistent) XSS`|The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)|
|`Reflected (Non-Persistent) XSS`|Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)|
|`DOM-based XSS`|Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)|
## XSS Testing Payloads
We can test whether the page is vulnerable to XSS with the following basic XSS payload:
```html
<script>alert(window.origin)</script>
```

```html
<plaintext>
```
- Will stop rendering the HTML code that comes after it and display it as plaintext

```html
<script>print()</script>
```
- Will pop up the browser print dialog, which is unlikely to be blocked by any browsers
## Stored XSS
`Stored XSS` / `Persistent XSS`: If our injected XSS payload gets stored in the back-end database and retrieved upon visiting the page, this means that our XSS attack is persistent and may affect any user that visits the page.

We can look at the previous section [[#XSS Testing Payloads]] to see if it is susceptible to XSS first. Once confirmed, we can move forward

To see whether the payload is persistent and stored on the back-end, we can refresh the page and see whether we get the alert again. If we do, we would see that we keep getting the alert even throughout page refreshes, confirming that this is indeed a `Stored/Persistent XSS` vulnerability. This is not unique to us, as any user who visits the page will trigger the XSS payload and get the same alert.
## Reflected XSS
There are two types of `Non-Persistent XSS` vulnerabilities: `Reflected XSS`, which gets processed by the back-end server, and `DOM-based XSS`, which is completely processed on the client-side and never reaches the back-end server. 

`Non-Persistent XSS` vulnerabilities are temporary and are not persistent through page refreshes. 

`Reflected XSS` vulnerabilities occur when our input reaches the back-end server and gets returned to us without being filtered or sanitized.
- There are many cases in which our entire input might get returned to us, like error messages or confirmation messages.

`But if the XSS vulnerability is Non-Persistent, how would we target victims with it?`

This depends on which HTTP request is used to send our input to the server. We can check this through the Firefox `Developer Tools` and we can put our `test` payload again and send it again.
### GET Requests
`GET` request sends their parameters and data as part of the URL. So, to target a user, we can send them a URL containing our payload. To get the URL, we can copy the URL from the URL bar in Firefox after sending our XSS payload, or we can right-click on the `GET` request in the `Network` tab and select `Copy>Copy URL`. Once the victim visits this URL, the XSS payload would execute.
## DOM XSS
Another `Non-Persistent`. While `reflected XSS` sends the input data to the back-end server through HTTP requests, DOM XSS is completely processed on the client-side through JavaScript. DOM XSS occurs when JavaScript is used to change the page source through the `Document Object Model (DOM)`.

If we open the `Network` tab in the Firefox Developer Tools, and re-add the `test` item, we would notice that no HTTP requests are being made.

The input parameter in the URL is using a hashtag `#` for the item we added, which means that this is a client-side parameter that is completely processed on the browser.

Furthermore, if we look at the page source we will notice that our `test` string is nowhere to be found. 
- This is because the JavaScript code is updating the page when we click the `Add` button, which is after the page source is retrieved by our browser, hence the base page source will not show our input, and if we refresh the page, it will not be retained
### Source & Sink
The `Source` is the JavaScript object that takes the user input, and it can be any input parameter like a URL parameter or an input field, as we saw above.

The `Sink` is the function that writes the user input to a DOM Object on the page. If the `Sink` function does not properly sanitize the user input, it would be vulnerable to an XSS attack.

Some of the commonly used JavaScript functions to write to DOM objects are:
- `document.write()`
- `DOM.innerHTML`
- `DOM.outerHTML`

Furthermore, some of the `jQuery` library functions that write to DOM objects are:
- `add()`
- `after()`
- `append()`

If a `Sink` function writes the exact input without any sanitization (like the above functions), and no other means of sanitization were used, then we know that the page should be vulnerable to XSS.
#### Example
We can look at the source code of the `To-Do` web application, and check `script.js`, and we will see that the `Source` is being taken from the `task=` parameter:
```javascript
var pos = document.URL.indexOf("task=");
var task = document.URL.substring(pos + 5, document.URL.length);
```

Right below these lines, we see that the page uses the `innerHTML` function to write the `task` variable in the `todo` DOM:
```javascript
document.getElementById("todo").innerHTML = "<b>Next Task:</b> " + decodeURIComponent(task);
```

So, we can see that we can control the input, and the output is not being sanitized, so this page should be vulnerable to DOM XSS.
### DOM Attacks
If we try a regular XSS payload, it will not execute because the `innerHTML` function does not allow the use of the `<script>` tags within it as a security feature.

Still, there are many other XSS payloads that do not contain `<script>` tags like: 
```html
<img src="" onerror=alert(window.origin)>
```
- Creates a new HTML image object, which has a `onerror` attribute that can execute JavaScript code when the image is not found. So, as we provided an empty image link (`""`), our code should always get executed without having to use `<script>` tags.

To target a user with this DOM XSS vulnerability, we can once again copy the URL from the browser and share it with them, and once they visit it, the JavaScript code should execute.
