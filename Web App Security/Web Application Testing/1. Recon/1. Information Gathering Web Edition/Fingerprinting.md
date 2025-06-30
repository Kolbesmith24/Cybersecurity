# Fingerprinting
Focuses on extracting technical details about the technologies powering a website or web application.
## Fingerprinting Techniques
There are several techniques used for web server and technology fingerprinting:

- `Banner Grabbing`: Banner grabbing involves analysing the banners presented by web servers and other services. These banners often reveal the server software, version numbers, and other details.
- `Analysing HTTP Headers`: HTTP headers transmitted with every web page request and response contain a wealth of information. The `Server` header typically discloses the web server software, while the `X-Powered-By` header might reveal additional technologies like scripting languages or frameworks.
- `Probing for Specific Responses`: Sending specially crafted requests to the target can elicit unique responses that reveal specific technologies or versions. For example, certain error messages or behaviours are characteristic of particular web servers or software components.
- `Analysing Page Content`: A web page's content, including its structure, scripts, and other elements, can often provide clues about the underlying technologies. There may be a copyright header that indicates specific software being used, for example.

A variety of tools exist that automate the fingerprinting process, combining various techniques to identify web servers, operating systems, content management systems, and other technologies:

|Tool|Description|Features|
|---|---|---|
|`Wappalyzer`|Browser extension and online service for website technology profiling.|Identifies a wide range of web technologies, including CMSs, frameworks, analytics tools, and more.|
|`BuiltWith`|Web technology profiler that provides detailed reports on a website's technology stack.|Offers both free and paid plans with varying levels of detail.|
|`WhatWeb`|Command-line tool for website fingerprinting.|Uses a vast database of signatures to identify various web technologies.|
|`Nmap`|Versatile network scanner that can be used for various reconnaissance tasks, including service and OS fingerprinting.|Can be used with scripts (NSE) to perform more specialised fingerprinting.|
|`Netcraft`|Offers a range of web security services, including website fingerprinting and security reporting.|Provides detailed reports on a website's technology, hosting provider, and security posture.|
|`wafw00f`|Command-line tool specifically designed for identifying Web Application Firewalls (WAFs).|Helps determine if a WAF is present and, if so, its type and configuration.|
## Fingerprinting Domains
Example domain: `inlanefreight.com`
### Banner Grabbing
first step is to gather information directly from the web server itself. We can do this using the `curl` command with the `-I` flag (or `--head`) to fetch only the HTTP headers, not the entire page content.
```bash
curl -I inlanefreight.com

curl IP_ADDRESS -v
```

The output will include the server banner, revealing the web server software and version number.

If it's also trying to redirect to another domain (`https://inlanefreight.com/`) so grab those banners too.

Keep following the different redirects from each new header.
### Wafw00f
Determines if the web application employs a WAF.

To detect the presence of a WAF, we'll use the `wafw00f` tool. To install `wafw00f`, you can use pip3:
```bash
pip3 install git+https://github.com/EnableSecurity/wafw00f
```

Once it's installed, pass the domain you want to check as an argument to the tool:
```bash
wafw00f inlanefreight.com
```
### Nikto
`Nikto` is a powerful open-source web server scanner. In addition to its primary function as a vulnerability assessment tool, `Nikto's` fingerprinting capabilities provide insights into a website's technology stack.

If you need to install it, you can run the following commands:
```bash
sudo apt update && sudo apt install -y perl
git clone https://github.com/sullo/nikto
cd nikto/program
chmod +x ./nikto.pl
```

To scan `inlanefreight.com` using `Nikto`, only running the fingerprinting modules, execute the following command:
```bash
nikto -h inlanefreight.com -Tuning b
```
- The `-h` flag specifies the target host. The `-Tuning b` flag tells `Nikto` to only run the Software Identification modules.

`Nikto` will then initiate a series of tests, attempting to identify outdated software, insecure files or configurations, and other potential security risks.