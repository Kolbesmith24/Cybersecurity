# FFUF - Fuzz Faster U Fool
## Testing for Local File Inclusion (LFI)

Local File Inclusion (LFI) vulnerabilities occur when an attacker can manipulate a web application to include local files from the server's file system. These vulnerabilities can lead to information disclosure, remote code execution, or other security issues.

### FFUF Command for LFI Testing

To test for LFI vulnerabilities using FFUF, you can use the following command:
```
ffuf -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt -u "http://URLHERE/?page=FUZZ" -fl 124
```

### Parameters:
- **`-w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt`**: Specifies the wordlist to use for the fuzzing process. In this case, we are using the `LFI-Jhaddix.txt` wordlist from the Seclists repository, which contains potential file paths commonly associated with LFI vulnerabilities.
  
- **`-u "http://URLHERE/?page=FUZZ"`**: The target URL. Replace `http://URLHERE/?page=FUZZ` with the actual URL of the application you are testing. The `FUZZ` keyword in the URL is where FFUF will inject entries from the wordlist to test for LFI vulnerabilities (e.g., `page=../../../etc/passwd`).

- **`-fl 124`**: This option filters the results based on the response length. The number `124` is used to identify specific file inclusion or error responses that often indicate a potential LFI vulnerability. Adjust the number based on observed response sizes in previous tests.

---

## Example Command Breakdown

Here is an example command with a placeholder URL:
```
ffuf -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt -u "http://example.com/?page=FUZZ" -fl 124
```

This command will:
1. Use the LFI wordlist (`LFI-Jhaddix.txt`) to test common file paths and names for potential LFI vulnerabilities.
2. Inject each wordlist entry into the `page` parameter of the URL (`http://example.com/?page=FUZZ`).
3. Filter the results based on a response length of 124 bytes, which is often indicative of a successful LFI vulnerability.

---
