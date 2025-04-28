# Deobfuscation - UMDCTF2025 Challenge Write-up
## Overview
We were provided with a file named `flag`, which was an ELF (Executable and Linkable Format) executable. Our objective was to analyze this binary, identify any hidden or encrypted data, and recover the correct password.

---

## Reconnaissance Phase

### 1. Initial Inspection of the File
Upon receiving the `flag` file, our first step was to perform a quick inspection.

Running basic file commands confirmed it was a compiled ELF binary:

```bash
file flag
```

![image](https://github.com/user-attachments/assets/a38a3222-6bc2-4ea7-9209-5a6adf1af165)


To quickly check for any readable text inside the binary, we tried using `cat`:

![image](https://github.com/user-attachments/assets/309270d7-6b68-49d5-a84b-1dc4b11705d4)


However, the output was mostly unreadable binary data, indicating that any useful information would not be available through a simple inspection.

---

### 2. Loading into GDB for Analysis

Since basic viewing didn't yield helpful information, we loaded the binary into **GDB** (GNU Debugger) for a more detailed analysis:

```bash
gdb ./flag
```

Once inside GDB, we disassembled the binary and examined different memory regions to find anything unusual. We looked for:
- Suspicious or structured patterns in memory.
- Embedded strings.
- Memory sections with static data.
    

Using the `x` (examine memory) command, we identified two important memory locations:
- One containing seemingly **encrypted data**.
- One containing potential **key data**.
    

---

## Exploitation Phase
### 1. Identifying the Important Sections
Through memory inspection, we identified two key regions:

- **Encrypted Data** at address `0x402000`:
    - A sequence of unreadable bytes.
        
![image](https://github.com/user-attachments/assets/61872cb7-ec09-4c2e-b63e-8398c590be0b)

    
- **Key Data** at address `0x402034`:
    
    - Another sequence of bytes that seemed random but was of similar length, suggesting it could serve as the decryption key.
        
![image](https://github.com/user-attachments/assets/aaf0cb77-264b-45e9-b9e3-58c4b735cc8f)

    

This pattern suggested that the encrypted data could be decrypted using the key data, likely through an XOR operation.

> **XOR Encryption**: XOR (exclusive OR) is a basic but effective symmetric encryption technique. XORing the same value twice with the same key returns the original value (`(A XOR B) XOR B = A`).

---

### 2. Devising a Decryption Strategy
Given the presence of encrypted data and a key, and recognizing the common use of XOR in simple CTF challenges, we formulated the following decryption plan:
- Read the encrypted data.
- Read the key data
- XOR each byte of the encrypted data with the corresponding byte of the key data.
- Assemble the resulting bytes into the plaintext password.
    

Since the two regions were aligned and of similar lengths, this further confirmed that a **byte-wise XOR** was the correct approach.

---

### 3. Automating Decryption with a Python Script
To automate the decryption process, we wrote a simple Python script.

The script:
- Defined the encrypted data bytes.
- Defined the key bytes.
- XORed each pair of corresponding bytes.
- Collected and printed the decrypted output as readable text.

Here’s the script used:

![image](https://github.com/user-attachments/assets/63b14821-89cc-4134-91fd-633f8ea23bfa)

---

### 4. Recovering the Password
Running the decryption script revealed the hidden password:

```
UMDCTF{r3v3R$E-i$_Th3_#B3ST#_4nT!-M@lW@r3_t3chN!Qu3}
```

![image](https://github.com/user-attachments/assets/1922a8bb-a75a-4818-afc6-6531315cd5ee)


Thus, we successfully solved the challenge.

---
