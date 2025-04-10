# BearcatCTF 2024 Challenge Writeups

These are writeups detailing the methodology and tools used to solve various challenges from BearcatCTF 2024. Each section explains the purpose of the challenge, step-by-step actions taken to solve it, tools used, and reasoning behind those choices.

---

## Simple Mystery (300 Points)

**Objective**: Analyze a file to discover the flag.

**Approach**:

1. **String Inspection with `strings`**:
   - The `strings` command in Unix-based systems scans files for human-readable text sequences. It's useful for identifying hidden messages or metadata in binaries or other file formats.
   - Command used: `strings <filename>`

2. **Making the File Executable**:
   - Changed file permissions using `chmod +x <filename>` to execute the file directly.

3. **Dynamic Analysis**:
   - Upon execution, the file's behavior suggested dynamic flag generation based on user input. This means the flag was not hardcoded but derived from processing input.

4. **Iterative Input Testing**:
   - By testing various inputs and reviewing the program’s output, the correct input was identified that produced the flag.

**Flag**: `BCCTF{SoLv1n6_TH3_C}`

---

## Brain_What?

**Objective**: Decode and execute Brainfuck code to reveal the flag.

**Approach**:

1. **Understanding Brainfuck**:
   - Brainfuck is an esoteric programming language with a minimalistic set of instructions (`+`, `-`, `>`, `<`, `[`, `]`, `.`, `,`). It operates on a memory array and is Turing complete.

2. **Code Retrieval**:
   - Downloaded `code.bf`, a file containing Brainfuck code.

3. **Code Execution with Online Tools**:
   - Used the [copy.sh Brainfuck Interpreter](https://copy.sh/brainfuck/) to run the Brainfuck code. This web-based tool executes Brainfuck programs and displays their output.
   - For code generation or visualization, the [Brainfuck Text Generator](https://copy.sh/brainfuck/text.html) was used.

4. **Flag Extraction**:
   - Output of the Brainfuck interpreter directly provided the flag.

---

## Needle in a Haystack

**Objective**: Analyze packet capture to find embedded flag data.

**Approach**:

1. **Packet Capture Analysis with Wireshark**:
   - Opened the provided `.pcap` file in Wireshark, a powerful tool for packet analysis and protocol inspection.

2. **Filtering for ICMP Traffic**:
   - Applied a display filter `icmp` to isolate Internet Control Message Protocol packets, which were mentioned in the challenge.

3. **Data Inspection**:
   - Within the ICMP packets marked "no response," base64-encoded strings were identified in the payload.

4. **Decoding with CyberChef**:
   - Used [CyberChef](https://gchq.github.io/CyberChef/) to decode the base64 strings and extract the flag.

---

## Fieldness

**Objective**: Investigate DNS traffic and understand DNS flag behavior.

**Approach**:

1. **DNS Flags Background**:
   - DNS protocol headers contain a flags field with several bits indicating query/response, truncation, recursion desired, recursion available, etc.
     - **QR (Query/Response)**: 1 for response, 0 for query.
     - **TC (Truncated)**: Indicates message truncation.
     - **RD (Recursion Desired)**: Request for recursive query resolution.

2. **Packet Inspection in Wireshark**:
   - Filtered for DNS packets with destination IP `67.227.193.163`.
   - Focused on A (Address) records which contain IPv4 address responses.

3. **Packet Flag Evaluation**:
   - Reviewed flag values in the DNS header to match the challenge hints.
   - Identified and isolated the packet(s) containing information that led to the flag.

---

## No Humans

**Objective**: Perform subdomain enumeration and server scanning to locate the flag.

**Approach**:

1. **Subdomain Enumeration with Sublist3r**:
   - Used `sublist3r.py` to attempt to discover subdomains.
   - This tool automates subdomain discovery using search engines, DNS queries, and APIs.
   - Syntax: `python sublist3r.py -d <domain>`
   - Result: No significant output.

2. **Nmap Scan**:
   - Switched to network scanning using Nmap to discover services and open ports.
   - Command: `nmap -sV <domain/IP>`

3. **Web Server Scanning with Nikto**:
   - Executed: `perl nikto.pl -host <IP>:<port>`
   - Nikto is a web vulnerability scanner that identifies common files, configurations, and potential security issues.

4. **Accessing Discovered Resource**:
   - Found the specific file containing the flag by visiting the discovered path.

---

## Packet Hunter

**Objective**: Extract a fragmented flag from network beacon messages.

**Approach**:

1. **Beacon Analysis**:
   - Opened the network capture and focused on communications from client IP `10.220.76.237`.

2. **Pattern Matching in Packets**:
   - Reviewed messages such as "Beacon Received" along with suspicious strings.
   - Used Wireshark’s display and follow stream features to trace specific message sequences.

3. **Flag Fragment Discovery**:
   - In packet number 35409 (timestamp: 1:17), found the partial flag string `BCCTF{c4nt_`.

4. **Full Flag Assembly**:
   - Continued reviewing subsequent packets to find and reconstruct the remaining segments of the flag.

