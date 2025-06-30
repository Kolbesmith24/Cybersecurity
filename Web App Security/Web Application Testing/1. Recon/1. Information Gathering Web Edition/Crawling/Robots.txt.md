# robots.txt
Acts as a virtual "`etiquette guide`" for bots, outlining which areas of a website they are allowed to access and which are off-limits.
## What is robots.txt?
A simple text file placed in the root directory of a website (e.g., `www.example.com/robots.txt`)
- Guidelines for how web crawlers should behave when visiting a website
### How robots.txt Works
The directives in robots.txt typically target specific user-agents, which are identifiers for different types of bots. For example, a directive might look like this:
```txt
User-agent: *
Disallow: /private/
```
- This directive tells all user-agents (`*` is a wildcard) that they are not allowed to access any URLs that start with `/private/`. Other directives can allow access to specific directories or files, set crawl delays to avoid overloading a server or provide links to sitemaps for efficient crawling.
### robots.txt Structure
Each record consists of two main components:
1. `User-agent`: This line specifies which crawler or bot the following rules apply to. A wildcard (`*`) indicates that the rules apply to all bots. Specific user agents can also be targeted, such as "Googlebot" (Google's crawler) or "Bingbot" (Microsoft's crawler).
2. `Directives`: These lines provide specific instructions to the identified user-agent.

Common directives include:

|Directive|Description|Example|
|---|---|---|
|`Disallow`|Specifies paths or patterns that the bot should not crawl.|`Disallow: /admin/` (disallow access to the admin directory)|
|`Allow`|Explicitly permits the bot to crawl specific paths or patterns, even if they fall under a broader `Disallow` rule.|`Allow: /public/` (allow access to the public directory)|
|`Crawl-delay`|Sets a delay (in seconds) between successive requests from the bot to avoid overloading the server.|`Crawl-delay: 10` (10-second delay between requests)|
|`Sitemap`|Provides the URL to an XML sitemap for more efficient crawling.|`Sitemap: https://www.example.com/sitemap.xml`|
### Analyzing robots.txt
Here's an example of a robots.txt file:
```txt
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /public/

User-agent: Googlebot
Crawl-delay: 10

Sitemap: https://www.example.com/sitemap.xml
```

This file contains the following directives:
- All user agents are disallowed from accessing the `/admin/` and `/private/` directories.
- All user agents are allowed to access the `/public/` directory.
- The `Googlebot` (Google's web crawler) is specifically instructed to wait 10 seconds between requests.
- The sitemap, located at `https://www.example.com/sitemap.xml`, is provided for easier crawling and indexing.

By analyzing this robots.txt, we can infer that the website likely has an admin panel located at `/admin/` and some private content in the `/private/` directory.