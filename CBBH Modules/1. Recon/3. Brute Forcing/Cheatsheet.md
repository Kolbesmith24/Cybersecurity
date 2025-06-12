## Brute-Forcing Tools

### Hydra
- Fast network login cracker
- Supports numerous protocols
- Uses parallel connections for speed
- Flexible and adaptable
- Relatively easy to use
```bash
hydra [-l LOGIN|-L FILE] [-p PASS|-P FILE] [-C FILE] -m MODULE [service://server[:PORT][/OPT]]
```
[[Hydra#Hydra]]

|Hydra Service|Service/Protocol|Description|Example Command|
|---|---|---|---|
|ftp|File Transfer Protocol (FTP)|Used to brute-force login credentials for FTP services, commonly used to transfer files over a network.|`hydra -l admin -P /path/to/password_list.txt ftp://192.168.1.100`|
|ssh|Secure Shell (SSH)|Targets SSH services to brute-force credentials, commonly used for secure remote login to systems.|`hydra -l root -P /path/to/password_list.txt ssh://192.168.1.100`|
|http-get/post|HTTP Web Services|Used to brute-force login credentials for HTTP web login forms using either GET or POST requests.|`hydra -l admin -P /path/to/password_list.txt 127.0.0.1 http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"`|

### Medusa

- Fast, massively parallel, modular login brute-forcer
- Supports a wide array of services

Code: bash

```bash
medusa [-h host|-H file] [-u username|-U file] [-p password|-P file] [-C file] -M module [OPT]
```

|Medusa Module|Service/Protocol|Description|Example Command|
|---|---|---|---|
|ssh|Secure Shell (SSH)|Brute force SSH login for the `admin` user.|`medusa -h 192.168.1.100 -u admin -P passwords.txt -M ssh`|
|ftp|File Transfer Protocol (FTP)|Brute force FTP with multiple usernames and passwords using 5 parallel threads.|`medusa -h 192.168.1.100 -U users.txt -P passwords.txt -M ftp -t 5`|
|rdp|Remote Desktop Protocol (RDP)|Brute force RDP login.|`medusa -h 192.168.1.100 -u admin -P passwords.txt -M rdp`|
|http-get|HTTP Web Services|Brute force HTTP Basic Authentication.|`medusa -h www.example.com -U users.txt -P passwords.txt -M http -m GET`|
|ssh|Secure Shell (SSH)|Stop after the first valid SSH login is found.|`medusa -h 192.168.1.100 -u admin -P passwords.txt -M ssh -f`|

### Custom Wordlists

Username Anarchy generates potential usernames based on a target's name.

|Command|Description|
|---|---|
|`username-anarchy Jane Smith`|Generate possible usernames for "Jane Smith"|
|`username-anarchy -i names.txt`|Use a file (`names.txt`) with names for input. Can handle space, CSV, or TAB delimited names.|
|`username-anarchy -a --country us`|Automatically generate usernames using common names from the US dataset.|
|`username-anarchy -l`|List available username format plugins.|
|`username-anarchy -f format1,format2`|Use specific format plugins for username generation (comma-separated).|
|`username-anarchy -@ example.com`|Append `@example.com` as a suffix to each username.|
|`username-anarchy --case-insensitive`|Generate usernames in case-insensitive (lowercase) format.|

CUPP (Common User Passwords Profiler) creates personalized password wordlists based on gathered intelligence.

|Command|Description|
|---|---|
|`cupp -i`|Generate wordlist based on personal information (interactive mode).|
|`cupp -w profiles.txt`|Generate a wordlist from a predefined profile file.|
|`cupp -l`|Download popular password lists like `rockyou.txt`.|

### Password Policy Filtering

Password policies often dictate specific requirements for password strength, such as minimum length, inclusion of certain character types, or exclusion of common patterns. `grep` combined with regular expressions can be a powerful tool for filtering wordlists to identify passwords that adhere to a given policy. Below is a table summarizing common password policy requirements and the corresponding `grep` regex patterns to apply:

|Policy Requirement|Grep Regex Pattern|Explanation|
|---|---|---|
|Minimum Length (e.g., 8 characters)|`grep -E '^.{8,}$' wordlist.txt`|`^` matches the start of the line, `.` matches any character, `{8,}` matches 8 or more occurrences, `$` matches the end of the line.|
|At Least One Uppercase Letter|`grep -E '[A-Z]' wordlist.txt`|`[A-Z]` matches any uppercase letter.|
|At Least One Lowercase Letter|`grep -E '[a-z]' wordlist.txt`|`[a-z]` matches any lowercase letter.|
|At Least One Digit|`grep -E '[0-9]' wordlist.txt`|`[0-9]` matches any digit.|
|At Least One Special Character|`grep -E '[!@#$%^&*()_+-=[]{};':"\,.<>/?]' wordlist.txt`|`[!@#$%^&*()_+-=[]{};':"\,.<>/?]` matches any special character (symbol).|
|No Consecutive Repeated Characters|`grep -E '(.)\1' wordlist.txt`|`(.)` captures any character, `\1` matches the previously captured character. This pattern will match any line with consecutive repeated characters. Use `grep -v` to invert the match.|
|Exclude Common Patterns (e.g., "password")|`grep -v -i 'password' wordlist.txt`|`-v` inverts the match, `-i` makes the search case-insensitive. This pattern will exclude any line containing "password" (or "Password", "PASSWORD", etc.).|
|Exclude Dictionary Words|`grep -v -f dictionary.txt wordlist.txt`|`-f` reads patterns from a file. `dictionary.txt` should contain a list of common dictionary words, one per line.|
|Combination of Requirements|`grep -E '^.{8,}$' wordlist.txt \| grep -E '[A-Z]'`|This command filters a wordlist to meet multiple password policy requirements. It first ensures that each word has a minimum length of 8 characters (`grep -E '^.{8,}$'`), and then it pipes the result into a second `grep` command to match only words that contain at least one uppercase letter (`grep -E '[A-Z]'`). This approach ensures the filtered passwords meet both the length and uppercase letter criteria.|
#### Example:
```bash
grep '[[:upper:]]' rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt
```
- Ensures there is 1 Upper-case, 1 lower-case, 1 digit, and the password length is at least 10