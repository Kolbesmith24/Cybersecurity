# Gobuster - Directory and File Discovery Tool

Gobuster is a tool used to discover hidden URLs, files, and directories within websites. It helps penetration testers and security professionals find unlinked or hidden resources that could potentially contain vulnerabilities or sensitive data.

## Syntax

The basic syntax for running Gobuster to discover directories and files on a web server is as follows:
```
gobuster dir -u http://IP_ADDRESS -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,js,txt,php,db,json,log -q -o OUTPUT_FILE
```

### Parameters:
- **`-u`**: The URL or IP address of the target site or web application you are attacking.
- **`-w`**: Path to the wordlist file that contains common directory and file names. For example, `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`.
- **`-x`**: Specifies the file extensions to check for. Multiple extensions can be provided, separated by commas (e.g., `html`, `js`, `txt`, `php`, `db`, `json`, `log`).
- **`-q`**: Runs the tool quietly, suppressing the banner and other noise, which makes the output cleaner and more concise.
- **`-o`**: Specifies the output file where the results will be saved.

---

## Example Command

Here is an example command for Gobuster that runs a directory scan on a website:
```
gobuster dir -u https://mainwp.com/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,js,txt,php,db,json,log -q -s "200,301,302,500" -b ""
```

### Parameters in this Example:
- **`-u https://mainwp.com/`**: The target URL we are scanning.
- **`-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`**: The wordlist to be used, which contains a list of directories and file names.
- **`-x html,js,txt,php,db,json,log`**: The file extensions to check for. This command will look for files with any of these extensions on the target website.
- **`-q`**: Runs the tool quietly, suppressing additional output.
- **`-s "200,301,302,500"`**: Tells Gobuster to only report responses with status codes 200 (OK), 301 (Moved Permanently), 302 (Found), or 500 (Internal Server Error). This helps filter out unnecessary results.
- **`-b ""`**: Tells Gobuster to ignore certain status codes (e.g., blank to ignore any).
  
---

## Conclusion

Gobuster is a fast and efficient tool for discovering hidden files and directories on a web server. By using it with appropriate wordlists and extensions, security professionals can uncover vulnerabilities, misconfigurations, or sensitive data that may otherwise be overlooked.
