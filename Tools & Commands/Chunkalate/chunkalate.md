# Chunkalate – PNG Repair and Analysis Tool

**Chunkalate** is a Python-based utility designed to inspect, diagnose, and repair corruption issues in PNG files. It is currently a work-in-progress and may have limited functionality or stability in certain cases, but it offers a unique set of features aimed at automating the process of analyzing and fixing malformed PNG files.

---
## Features
Chunkalate's current and intended capabilities include:
- **Full PNG metadata inspection**: Extracts all available information from PNG files.
- **Repair functions**:
    - Fixes corrupted or missing **magic headers** and **footers**
    - Corrects **chunk lengths**, **chunk names**, and **chunk CRCs**
    - Adjusts incorrect **image dimensions**
    - Repairs issues related to **line feed conversions**
    - Smart CRC fixing based on detected errors

- **Chunk management**:
    - Identifies and corrects misplaced critical chunks (currently supports IHDR)
    - Replaces **critical missing chunks** with safe defaults
    - Attempts **brute-force recovery** of corrupted data chunks
    
- **Versioned saving**: Each modification is saved to a new file for traceability
- **Readable summary**: Provides a detailed report of all actions taken
- **User-friendly interactions**: Interactive pauses and clean screen output

---

### Usage
To use Chunkalate, run the script with the appropriate command-line arguments:
```bash
python Chunklate.py [options]
```
#### Options

|Option|Description|
|---|---|
|`-h, --help`|Display help message and exit|
|`-f FILE, --file FILE`|Specify the path to the PNG file to analyze/repair|
|`-c, --clear`|Clear the terminal screen after each save|
|`-p, --pause`|Pause execution after each save for manual inspection|
|`-d, --debug`|Enable debug mode (prints internal debugging information)|
|`-dp, --pause-debug`|Pause execution at debug checkpoints|
|`-ep, --pause-error`|Pause when an error is encountered|
|`-sp, --pause-dialogue`|Pause during interactive prompts/dialogues|
|`-stfu, --shut-the-fuck-up`|Minimal output mode (suppresses most logs)|
|`-a, --auto`|Automatically choose actions when prompted|

---

### Example
To run Chunkalate on a file and allow it to auto-fix detected issues with minimal output:
```bash
python Chunklate.py -f corrupted_image.png -a -stfu
```

---
