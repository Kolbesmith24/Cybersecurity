# Introduction to Sessions
A user session can be defined as a sequence of requests originating from the same client and the associated responses during a specific time period. Modern web applications need to maintain user sessions to keep track of information and status about each user. User sessions facilitate the assignment of access or authorization rights, localization settings, etc., while users interact with an application, pre, and post-authentication.
## Session Identifier Security
A unique **session identifier** (Session ID) or token is the basis upon which user sessions are generated and distinguished.

We should clarify that if an attacker obtains a session identifier, this can result in session hijacking, where the attacker can essentially impersonate the victim in the web application.

An attacker can obtain a session identifier through a multitude of techniques, not all of which include actively attacking the victim. A session identifier can also be:
- Captured through passive traffic/packet sniffing
- Identified in logs
- Predicted
- Brute Forced

A session identifier's security level depends on its:
- `Validity Scope` (a secure session identifier should be valid for one session only)
- `Randomness` (a secure session identifier should be generated through a robust random number/string generation algorithm so that it cannot be predicted)
- `Validity Time` (a secure session identifier should expire after a certain amount of time)

The established programming technologies (PHP, JSP, etc.) generate session identifiers that "comply" with the above _validity scope_, _randomness_, and _validity time_ requirements. If you come across a custom session identifier generation implementation, proceed with extreme caution and be as exhaustive as possible in tests.

A session identifier's security level also depends on the location where it is stored:
- `URL`: If this is the case, the HTTP _Referer_ header can leak a session identifier to other websites. In addition, browser history will also contain any session identifier stored in the URL.
- `HTML`: If this is the case, the session identifier can be identified in both the browser's cache memory and any intermediate proxies
- `sessionStorage`: SessionStorage is a browser storage feature introduced in HTML5. Session identifiers stored in sessionStorage can be retrieved as long as the tab or the browser is open. In other words, sessionStorage data gets cleared when the _page session_ ends. Note that a page session survives over page reloads and restores.
- `localStorage`: LocalStorage is a browser storage feature introduced in HTML5. Session identifiers stored in localStorage can be retrieved as long as localStorage does not get deleted by the user. This is because data stored within localStorage will not be deleted when the browser process is terminated, with the exception of "private browsing" or "incognito" sessions where data stored within localStorage are deleted by the time the last tab is closed.

Session identifiers that are managed with no server interference or that do not follow the secure "characteristics" above should be reported as weak.
## Session Attacks
- `Session Hijacking`: In session hijacking attacks, the attacker takes advantage of insecure session identifiers, finds a way to obtain them, and uses them to authenticate to the server and impersonate the victim.
- `Session Fixation`: Session Fixation occurs when an attacker can fixate a (valid) session identifier. As you can imagine, the attacker will then have to trick the victim into logging into the application using the aforementioned session identifier. If the victim does so, the attacker can proceed to a Session Hijacking attack (since the session identifier is already known).
- `XSS (Cross-Site Scripting)` <-- With a focus on user sessions
- `CSRF (Cross-Site Request Forgery)`: Cross-Site Request Forgery (CSRF or XSRF) is an attack that forces an end-user to execute inadvertent actions on a web application in which they are currently authenticated. This attack is usually mounted with the help of attacker-crafted web pages that the victim must visit or interact with. These web pages contain malicious requests that essentially inherit the identity and privileges of the victim to perform an undesired function on the victim's behalf.
- `Open Redirects` <-- With a focus on user sessions: An Open Redirect vulnerability occurs when an attacker can redirect a victim to an attacker-controlled site by abusing a legitimate application's redirection functionality. In such cases, all the attacker has to do is specify a website under their control in a redirection URL of a legitimate website and pass this URL to the victim. As you can imagine, this is possible when the legitimate application's redirection functionality does not perform any kind of validation regarding the websites which the redirection points to.
