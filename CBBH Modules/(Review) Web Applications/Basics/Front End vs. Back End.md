# Front End vs. Back End
## Front End
Contains the user's components directly through their web browser (client-side). These components make up the source code of the web page.

This includes everything that the user sees and interacts with.
## Back End
Drives all of the core web application functionalities, all of which is executed at the back end server, which processes everything required for the web application to run correctly. 

It is the part we may never see or directly interact with, but a website is just a collection of static web pages without a back end.

Four main back end components for web applications:

|**Component**|**Description**|
|---|---|
|`Back end Servers`|The hardware and operating system that hosts all other components and are usually run on operating systems like `Linux`, `Windows`, or using `Containers`.|
|`Web Servers`|Web servers handle HTTP requests and connections. Some examples are `Apache`, `NGINX`, and `IIS`.|
|`Databases`|Databases (`DBs`) store and retrieve the web application data. Some examples of relational databases are `MySQL`, `MSSQL`, `Oracle`, `PostgreSQL`, while examples of non-relational databases include `NoSQL` and `MongoDB`.|
|`Development Frameworks`|Development Frameworks are used to develop the core Web Application. Some well-known frameworks include `Laravel` (`PHP`), `ASP.NET` (`C#`), `Spring` (`Java`), `Django` (`Python`), and `Express` (`NodeJS JavaScript`).|
![[backend-server.jpg]]It is also possible to host each component of the back end on its own isolated server, or in isolated containers, by utilizing services such as [Docker](https://www.docker.com).

To maintain logical separation and mitigate the impact of vulnerabilities, different components of the web application, such as the database, can be installed in one Docker container, while the main web application is installed in another, thereby isolating each part from potential vulnerabilities that may affect the other container(s).
## Securing Front/Back End
Back end could still be exploited by various injection attacks even though we don't have access to the back end code, for example.

The `top 20` most common mistakes web developers make that are essential for us as penetration testers are:

|**No.**|**Mistake**|
|---|---|
|`1.`|Permitting Invalid Data to Enter the Database|
|`2.`|Focusing on the System as a Whole|
|`3.`|Establishing Personally Developed Security Methods|
|`4.`|Treating Security to be Your Last Step|
|`5.`|Developing Plain Text Password Storage|
|`6.`|Creating Weak Passwords|
|`7.`|Storing Unencrypted Data in the Database|
|`8.`|Depending Excessively on the Client Side|
|`9.`|Being Too Optimistic|
|`10.`|Permitting Variables via the URL Path Name|
|`11.`|Trusting third-party code|
|`12.`|Hard-coding backdoor accounts|
|`13.`|Unverified SQL injections|
|`14.`|Remote file inclusions|
|`15.`|Insecure data handling|
|`16.`|Failing to encrypt data properly|
|`17.`|Not using a secure cryptographic system|
|`18.`|Ignoring layer 8|
|`19.`|Review user actions|
|`20.`|Web Application Firewall misconfigurations|

These mistakes lead to the [OWASP Top 10](https://owasp.org/www-project-top-ten/) vulnerabilities:

|**No.**|**Vulnerability**|
|---|---|
|`1.`|Broken Access Control|
|`2.`|Cryptographic Failures|
|`3.`|Injection|
|`4.`|Insecure Design|
|`5.`|Security Misconfiguration|
|`6.`|Vulnerable and Outdated Components|
|`7.`|Identification and Authentication Failures|
|`8.`|Software and Data Integrity Failures|
|`9.`|Security Logging and Monitoring Failures|
|`10.`|Server-Side Request Forgery (SSRF)|