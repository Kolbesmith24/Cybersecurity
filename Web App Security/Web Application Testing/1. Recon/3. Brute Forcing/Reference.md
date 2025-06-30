# How Brute Forcing Works
The brute force process can be visualized as follows:
![Flowchart showing process: Start, generate combination, apply combination, check success, access granted if successful.](https://academy.hackthebox.com/storage/modules/57/1n.png)
1. `Start`: The attacker initiates the brute force process, often with the aid of specialized software.
2. `Generate Possible Combination`: The software generates a potential password or key combination based on predefined parameters, such as character sets and length.
3. `Apply Combination`: The generated combination is attempted against the target system, such as a login form or encrypted file.
4. `Check if Successful`: The system evaluates the attempted combination. If it matches the stored password or key, access is granted. Otherwise, the process continues.
5. `Access Granted`: The attacker gains unauthorized access to the system or data.
6. `End`: The process repeats, generating and testing new combinations until either the correct one is found or the attacker gives up.
## Types of Brute Forcing
|Method|Description|Example|Best Used When...|
|---|---|---|---|
|`Simple Brute Force`|Systematically tries all possible combinations of characters within a defined character set and length range.|Trying all combinations of lowercase letters from 'a' to 'z' for passwords of length 4 to 6.|No prior information about the password is available, and computational resources are abundant.|
|`Dictionary Attack`|Uses a pre-compiled list of common words, phrases, and passwords.|Trying passwords from a list like 'rockyou.txt' against a login form.|The target will likely use a weak or easily guessable password based on common patterns.|
|`Hybrid Attack`|Combines elements of simple brute force and dictionary attacks, often appending or prepending characters to dictionary words.|Adding numbers or special characters to the end of words from a dictionary list.|The target might use a slightly modified version of a common password.|
|`Credential Stuffing`|Leverages leaked credentials from one service to attempt access to other services, assuming users reuse passwords.|Using a list of usernames and passwords leaked from a data breach to try logging into various online accounts.|A large set of leaked credentials is available, and the target is suspected of reusing passwords across multiple services.|
|`Password Spraying`|Attempts a small set of commonly used passwords against a large number of usernames.|Trying passwords like 'password123' or 'qwerty' against all usernames in an organization.|Account lockout policies are in place, and the attacker aims to avoid detection by spreading attempts across multiple accounts.|
|`Rainbow Table Attack`|Uses pre-computed tables of password hashes to reverse hashes and recover plaintext passwords quickly.|Pre-computing hashes for all possible passwords of a certain length and character set, then comparing captured hashes against the table to find matches.|A large number of password hashes need to be cracked, and storage space for the rainbow tables is available.|
|`Reverse Brute Force`|Targets a single password against multiple usernames, often used in conjunction with credential stuffing attacks.|Using a leaked password from one service to try logging into multiple accounts with different usernames.|A strong suspicion exists that a particular password is being reused across multiple accounts.|
|`Distributed Brute Force`|Distributes the brute forcing workload across multiple computers or devices to accelerate the process.|Using a cluster of computers to perform a brute-force attack significantly increases the number of combinations that can be tried per second.|The target password or key is highly complex, and a single machine lacks the computational power to crack it within a reasonable timeframe.|
# Brute Force Attacks
The following formula determines the total number of possible combinations for a password:
```mathml
Possible Combinations = Character Set Size^Password Length
```
## Dictionary Attacks
|Feature|Dictionary Attack|Brute Force Attack|Explanation|
|---|---|---|---|
|`Efficiency`|Considerably faster and more resource-efficient.|Can be extremely time-consuming and resource-intensive.|Dictionary attacks leverage a pre-defined list, significantly narrowing the search space compared to brute-force.|
|`Targeting`|Highly adaptable and can be tailored to specific targets or systems.|No inherent targeting capability.|Wordlists can incorporate information relevant to the target (e.g., company name, employee names), increasing the success rate.|
|`Effectiveness`|Exceptionally effective against weak or commonly used passwords.|Effective against all passwords given sufficient time and resources.|If the target password is within the dictionary, it will be swiftly discovered. Brute force, while universally applicable, can be impractical for complex passwords due to the sheer volume of combinations.|
|`Limitations`|Ineffective against complex, randomly generated passwords.|Often impractical for lengthy or highly complex passwords.|A truly random password is unlikely to appear in any dictionary, rendering this attack futile. The astronomical number of possible combinations for lengthy passwords can make brute-force attacks infeasible.|
Consider a hypothetical scenario where an attacker targets a company's employee login portal. The attacker might construct a specialized wordlist that incorporates the following:

- Commonly used, weak passwords (e.g., "password123," "qwerty")
- The company name and variations thereof
- Names of employees or departments
- Industry-specific jargon

By deploying this targeted wordlist in a dictionary attack, the attacker significantly elevates their likelihood of successfully cracking employee passwords compared to a purely random brute-force endeavor.
## Hybrid Attacks
Unfortunately, a widespread and insecure practice among users is making minor modifications to their passwords when forced to change them. This often manifests as appending a number or a special character to the end of the current password. For instance, a user might have an initial password like "Summer2023" and then, when prompted to update it, change it to "Summer2023!" or "Summer2024."
### Hybrid Attacks in Action
Let's illustrate this with a practical example. Consider an attacker targeting an organization known to enforce regular password changes.

The attacker begins by launching a dictionary attack, using a wordlist curated with common passwords, industry-specific terms, and potentially personal information related to the organization or its employees. This phase attempts to quickly identify any low-hanging fruit - accounts protected by weak or easily guessable passwords.

However, if the dictionary attack proves unsuccessful, the hybrid attack seamlessly transitions into a brute-force mode. Instead of randomly generating password combinations, it strategically modifies the words from the original wordlist, appending numbers, special characters, or even incrementing years, as in our "Summer2023" example.

Let's consider a scenario where you have access to a common passwords wordlist, and you're targeting an organization with the following password policy:

- Minimum length: 8 characters
- Must include:
    - At least one uppercase letter
    - At least one lowercase letter
    - At least one number

To extract only the passwords that adhere to this policy, we can leverage the powerful command-line tools available on most Linux/Unix-based systems by default, specifically `grep` paired with regex. We are going to use the [darkweb2017-top10000.txt](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/darkweb2017_top-10000.txt) password list for this. First, download the wordlist
```bash
wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/darkweb2017_top-10000.txt
```

Next, we need to start matching that wordlist to the password policy.
```bash
grep -E '^.{8,}$' darkweb2017-top10000.txt > darkweb2017-minlength.txt
```

This initial `grep` command targets the core policy requirement of a minimum password length of 8 characters. The regular expression `^.{8,}$` acts as a filter, ensuring that only passwords containing at least 8 characters are passed through and saved in a temporary file named `darkweb2017-minlength.txt`.
```bash
grep -E '[A-Z]' darkweb2017-minlength.txt > darkweb2017-uppercase.txt
```

Building upon the previous filter, this `grep` command enforces the policy's demand for at least one uppercase letter. The regular expression `[A-Z]` ensures that any password lacking an uppercase letter is discarded, further refining the list saved in `darkweb2017-uppercase.txt`.
```bash
grep -E '[a-z]' darkweb2017-uppercase.txt > darkweb2017-lowercase.txt
```

Maintaining the filtering chain, this `grep` command ensures compliance with the policy's requirement for at least one lowercase letter. The regular expression `[a-z]` serves as the filter, keeping only passwords that include at least one lowercase letter and storing them in `darkweb2017-lowercase.txt`.
```bash
grep -E '[0-9]' darkweb2017-lowercase.txt > darkweb2017-number.txt
```

This last `grep` command tackles the policy's numerical requirement. The regular expression `[0-9]` acts as a filter, ensuring that passwords containing at least one numerical digit are preserved in `darkweb2017-number.txt`.
```bash
wc -l darkweb2017-number.txt
```
## Credential Stuffing: Leveraging Stolen Data for Unauthorized Access
Exploit the unfortunate reality that many users reuse passwords across multiple online accounts. This pervasive practice, often driven by the desire for convenience and the challenge of managing numerous unique credentials, creates a fertile ground for attackers to exploit.