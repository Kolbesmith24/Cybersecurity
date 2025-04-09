🔎 Burp Suite Notes

Burp Suite is a comprehensive tool used for web application security testing. This reference provides a breakdown of its core modules and usage, tailored for penetration testers and bug bounty hunters.
# Core Modules
## Proxy

    Purpose: Intercepts and modifies HTTP/S requests and responses between the client and server.

    Usage: Crucial for observing and manipulating live traffic to identify security flaws.

## Repeater

    Purpose: Modify and resend HTTP/S requests manually.

    Common Use Cases:

        SQL Injection payload testing

        Testing endpoint behavior

Repeater Interface Overview:

    Request List: Manage multiple requests simultaneously.

    Request Controls: Send, cancel, and navigate request history.

    Request/Response View: Modify requests and inspect responses.

    Layout Options: Customize UI layout.

    Inspector: Intuitive analysis and editing of request elements.

    Target Field: Automatically populated from Proxy or manually set.

    Shortcut: Send a request from Proxy to Repeater using Ctrl + Shift + R.

## Intruder

    Purpose: Automates sending of custom HTTP requests for brute-force or fuzzing attacks.

    Note: Burp Suite Community Edition has rate-limiting for Intruder.

Intruder Sub-Tabs:

    Positions

        Select the attack type and insert payload markers (§).

        Use:

            Add § to insert manual positions

            Clear § to reset all

            Auto § to auto-detect positions

    Payloads

        Define payload lists (e.g., wordlists)

        Options include:

            Pre-processing rules

            Match/replace

            Regex filters

    Resource Pool (Pro Only)

        Allocates resources across automated tasks.

    Settings

        Controls for flagging responses, redirect handling, and result behavior.

### Attack Types:
Type	Description
Sniper	Single payload, single position (ideal for brute-force, fuzzing).
Battering Ram	Same payload used across all positions simultaneously.
Pitchfork	Multiple payload sets, one per position, tested in parallel.
Cluster Bomb	Multiple payload sets, each tested across all combinations (combinatorial).
## Decoder

    Purpose: Encode, decode, and transform data.

    Capabilities:

        Encode payloads before transmission

        Decode captured values

        Generate hashes

        Use Smart Decode (like CyberChef's "Magic")

## Comparer

    Purpose: Byte or word-level comparison of data.

    Usage: Quickly identify differences in response content or tokens.

## Sequencer

    Purpose: Analyze randomness of tokens (e.g., session IDs).

    Usage: Detect insecure random generation that may lead to token prediction attacks.

## Extensions Interface
Sections:

    Extensions List: Enable/disable active extensions.

    Manage Extensions:

        Add: Install custom or third-party modules

        Remove: Uninstall existing extensions

        Up/Down: Control execution order

    Details, Output, Errors:

        Debug output and error handling for extensions

## BApp Store

    Purpose: Official extension marketplace for Burp Suite.

    Languages Supported:

        Java (native integration)

        Python (requires Jython)

## Summary of Shortcuts & Tools
Module	Purpose	Key Shortcut / Notes
Proxy	Intercept & modify traffic	Central to all traffic manipulation
Repeater	Manual testing of requests	Ctrl + Shift + R to send from Proxy
Intruder	Automate attacks (fuzzing, bruteforce)	Limited in Community Edition
Decoder	Transform encoded/hashed data	Smart Decode available
Comparer	Visual diff between two requests	Fast diffing tool
Sequencer	Randomness analysis of tokens	Entropy analysis built-in
Extensions	Extend Burp's functionality	Custom & community-written support
BApp Store	Discover new extensions	Java/Python support

