# WPScan Overview
## Using WPScan
[WPScan](https://github.com/wpscanteam/wpscan) is an automated WordPress scanner and enumeration tool. It determines if the various themes and plugins used by a WordPress site are outdated or vulnerable. It is installed by default on Parrot OS but can also be installed manually with `gem`.
```bash
gem install wpscan
```
- Once the installation completes, we can issue a command such as `wpscan --hh` to verify the installation.

There are various enumeration options that can be specified, such as vulnerable plugins, all plugins, user enumeration, and more. It is important to understand all of the options available to us and fine-tune the scanner depending on the goal.

WPScan can pull in vulnerability information from external sources to enhance our scans. We can obtain an API token from [WPVulnDB](https://wpvulndb.com), which is used by WPScan to scan for vulnerability and exploit proof of concepts (POC) and reports. The free plan allows up to 50 requests per day. To use the WPVulnDB database, just create an account and copy the API token from the users page. This token can then be supplied to WPScan using the `--api-token` parameter.
# WPScan Enumeration
The `--enumerate` flag is used to enumerate various components of the WordPress application such as plugins, themes, and users. 

By default, WPScan enumerates vulnerable plugins, themes, users, media, and backups. However, specific arguments can be supplied to restrict enumeration to specific components. 
- For example, all plugins can be enumerated using the arguments `--enumerate ap`. Let's run a normal enumeration scan against a WordPress website.
### WPScan Enumeration
```bash
wpscan --url http://STMIP:STMPO --enumerate ap
```
- `-e` - short version of `--enumerate`) option
- `ap` - short for `all plugins`

WPScan uses various passive and active methods to determine versions and vulnerabilities.