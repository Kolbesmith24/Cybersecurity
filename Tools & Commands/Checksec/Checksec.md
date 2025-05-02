# Quick Reference
## Usage
To check the security features of a binary, you can run `checksec` on the binary like this:
```bash
checksec --file=./FILENAME
```

This will show if the various protections are enabled or not.
# What is it?
`checksec` is a security tool that is commonly used to check for various security mechanisms that are enabled or disabled in a binary or executable. It is typically used to evaluate whether a program has certain protections enabled that can make it more difficult to exploit vulnerabilities, such as buffer overflows.
### Key Security Protections Checked by `checksec`:
1. **Stack Canary (Canary Value)**:
    - This is a value placed on the stack to detect stack buffer overflows. If the value is overwritten during a buffer overflow, the program can detect the attack and terminate.
    - The `-fstack-protector` compiler flag enables stack protection.
        
2. **Position Independent Executable (PIE)**:
    - PIE allows the program to be loaded at random addresses in memory, making it more difficult for an attacker to predict the location of certain functions or buffers.
    - The `-fPIE` compiler flag enables PIE.
        
3. **Data Execution Prevention (DEP)**:
    - Also known as Non-Executable (NX) stack, this prevents code from being executed on certain regions of memory, such as the stack or heap, where user data resides.
    - It helps prevent exploits like shellcode injection.
        
4. **Relocation Read-Only (RELRO)**:
    - This technique prevents the modification of function pointers in a program. There are two levels:
        - **Partial RELRO**: Protects the GOT (Global Offset Table) from being overwritten.
        - **Full RELRO**: Prevents any relocation from happening during execution and can make some exploits harder, especially with respect to GOT overwriting.
            
5. **Stack Smashing Protection**:
    - Another form of stack protection that can help defend against stack buffer overflows.
### Output Example:
Here's an example of what the output might look like:
```bash
RELRO           Full RELRO
Stack Canary    No stack protector found
NX              NX enabled
PIE             No PIE (0x400000)
```
- **RELRO**: Full or Partial (if any)
- **Stack Canary**: Whether stack protection is enabled
- **NX**: If Data Execution Prevention is enabled
- **PIE**: Whether the program is a Position Independent Executable
### Why is it useful?
By running `checksec`, you can quickly determine which protections are in place, which can help you design an exploit. For instance:
- If `PIE` is not enabled, you can rely on fixed addresses when creating a buffer overflow exploit.
- If `NX` is enabled, you might need to bypass data execution protection, often by using techniques like ROP (Return-Oriented Programming).

## Exploiting improper protections
#### Vulnerable to overflow:
It is vulnerable to overflow if:
1. PIE is disabled
2. There's no canary

We can then proceed with the following strategy:
1. **Overflow the stack** until we control EIP.
2. **Overwrite EIP** with the address of `specified_function()`.
3. **Trigger the shell**.
##### If the overflow happens through `scanf("%lf")`
We must carefully craft our input as **doubles (floating-point numbers)**.

Why?
- `scanf("%lf", &var)` expects to read a **`double`**.
- A `double` in C is **8 bytes** (64 bits) — **always**.
- So every time you send input through `scanf("%lf")`, it reads **8 bytes** from your input, interpreting them as a floating-point number.
- **If you provide too much input**, it can **overwrite neighboring memory**

**If you input something that's _not_ a valid double**, `scanf` will either:
- fail to parse it, or
- leave `var` unchanged, or
- cause undefined behavior (rarely).
