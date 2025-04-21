# Exploring Windows Registry Files with RegRipper & RegLookup

Windows registry hives can contain critical forensic artifacts for incident response, malware analysis, and CTF challenges. This guide covers how to extract and analyze data using **RegRipper** and **RegLookup**, two essential tools in any forensic analyst’s toolkit.

---
##  Reconnaissance & Extraction Workflow
Before diving into a full forensic analysis, it’s helpful to gather basic information from Windows registry hives like `NTUSER.DAT`, `SOFTWARE`, `SYSTEM`, and `SAM`. These files often contain indicators of compromise, recent user activity, and even CTF flags hidden in plaintext.

---
## RegRipper — Quick Start Guide
RegRipper is a Perl-based framework that parses Windows registry hives using purpose-built plugins.
### Installation
```bash
git clone https://github.com/keydet89/RegRipper3.0.git
cd RegRipper3.0
chmod +x rip.pl
```
### Basic Usage
#### 1. Run Common Plugins
Use specific plugins to target high-value data sources:
```bash
./rip.pl -r NTUSER.DAT -f userassist
./rip.pl -r NTUSER.DAT -f recentdocs
./rip.pl -r NTUSER.DAT -f run
./rip.pl -r SOFTWARE -f uninstall
./rip.pl -r SYSTEM -f services
./rip.pl -r SAM -f sam
```
- `-r` = Path to registry file
- `-f` = Plugin to use
#### 2. Grep for the Flag (CTFs)
You can run the `all` plugin and grep for common CTF keywords or flag formats:
```bash
./rip.pl -r NTUSER.DAT -f all | grep -Ei 'flag|ctf|{.*}'
```

Alternatively, pipe plugin output to a text file and search offline:
```bash
grep -iE "flag|ctf|{.*}" output.txt
```

---
### Plugin Reference

#### NTUSER.DAT Plugins

|Plugin|Description|
|---|---|
|`userassist`|Tracks executed programs (ROT13 encoded)|
|`muicache`|Recently launched programs|
|`typedurls`|URLs entered in Internet Explorer|
|`run`|Auto-run programs on user login|
|`recentdocs`|Recently opened documents|
|`shellbags`|Folder access tracking in Explorer|
|`wordwheelquery`|Search bar terms|
|`bam`|Background Activity Moderator execution|
|`amcache`|App execution cache (Windows 8+)|
|`jumplist`|Recently accessed files via Jump Lists|

#### SOFTWARE Hive Plugins

|Plugin|Description|
|---|---|
|`uninstall`|Installed applications|
|`apppaths`|App full paths|
|`product`|OS install info|
|`winlogon`|Auto-login and shell settings|
|`internetsettings`|Proxy and IE settings|
|`services`|Registered services|
|`timezone`|System time configuration|
|`office`|Microsoft Office metadata|

#### SYSTEM Hive Plugins

| Plugin         | Description                          |
| -------------- | ------------------------------------ |
| `services`     | Services and drivers                 |
| `computername` | System hostname                      |
| `timezone`     | System timezone                      |
| `controlset`   | Boot/config settings                 |
| `networklist`  | Connected networks                   |
| `mountdev`     | Mounted devices                      |
| `syskey`       | System key and password-related info |

#### SAM Hive Plugins

|Plugin|Description|
|---|---|
|`sam`|User accounts & last login timestamps|
|`samparse`|Detailed SAM parsing|
|`usergroups`|Group membership data|

#### Amcache.hve (Windows 8+)

|Plugin|Description|
|---|---|
|`amcache`|Application execution history|
|`filepaths`|File path metadata for executed programs|

#### General / Multi-Hive Plugins

|Plugin|Description|
|---|---|
|`all`|Runs all available plugins|
|`recentapps`|Recently used apps|
|`usbdevices`|Connected USB devices|
|`usbstor`|USB storage mount info|

#### List All Available Plugins

```bash
./rip.pl -l
```

---

## RegLookup — Quick Start Guide
RegLookup is a command-line utility for extracting and analyzing raw registry data.
### Installation
```bash
sudo apt install reglookup
```
### Basic Usage
#### 1. Dump Registry Hive Contents
```bash
reglookup /path/to/NTUSER.DAT > dump.txt
```
#### 2. Search for Flags and Patterns
```bash
grep -iE "flag|ctf|{.*}" dump.txt
```
#### 3. Timeline Analysis
View a chronological list of modified keys, useful for detecting recent activity:
```bash
reglookup-timeline NTUSER.DAT | tail -n 20
```
#### 4. Query Specific Values or Keys
For example, to extract timezone information:
```bash
reglookup /path/to/SYSTEM | grep -i timezone
```

Or to see a timeline view of SYSTEM hive activity:
```bash
reglookup-timeline /path/to/SYSTEM
```

---
## Summary

- **RegRipper** is plugin-based and ideal for targeted forensic parsing.
- **RegLookup** is great for full-dump parsing and chronological analysis.
