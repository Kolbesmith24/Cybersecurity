# Metasploit Overview
. **msfconsole**: The main command-line interface.
. **Modules**: supporting modules such as exploits, scanners, payloads, etc.
. **Tools**: Stand-alone tools that will help vulnerability research, vulnerability assessment, or penetration testing. 
# Metasploit Main Components
### Auxiliary
Any supporting module, such as scanners, crawlers and fuzzers, can be found here.
### Encoders
Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.

Signature-based antivirus and security solutions have a database of known threats. They detect threats by comparing suspicious files to this database and raise
an alert if there is a match. Thus encoders can have a limited success rate as antivirus solutions can perform additional checks.
### Evasion
While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software. On the other hand, "evasion" modules will
try that, with more or less success.
### Exploits
Exploits, neatly organized by target system.
### NOPS
NOPs (No OPeration) do nothing, literally. They are represented in the Intel x86 CPU family they are represented with 0x90, following which the CPU will do
nothing for one cycle. They are often used as a buffer to achieve consistent payload sizes.
### Payloads
Payloads are codes that will run on the target system.

Exploits will leverage a vulnerability on the target system, but to achieve the desired result, we will need a payload. Examples could be; getting a shell, loading a malware or backdoor to the target system, running a command, or launching calc.exe as a proof of concept to add to the penetration test report. 

You will see four different directories under payloads: adapters, singles, stagers and stages.

- **Adapters:** An adapter wraps single payloads to convert them into different formats. For example, a normal single payload can be wrapped inside a Powershell adapter, which will make a single powershell command that will execute the payload.
- **Singles:** Self-contained payloads (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.
- **Stagers:** Responsible for setting up a connection channel between Metasploit and the target system. Useful when working with staged payloads. "Staged payloads" will first upload a stager on the target system then download the rest of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.
- **Stages:** Downloaded by the stager. This will allow you to use larger sized payloads.
### Post
Post modules will be useful on the final stage of the penetration testing process listed above, post-exploitation.
