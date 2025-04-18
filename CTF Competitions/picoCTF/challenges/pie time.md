### CTF Writeup: Pie Time Challenge

#### Overview:
The "Pie Time" CTF challenge involved reverse engineering a binary executable. The goal was to find a specific address (the flag) by manipulating the program's flow using the `main()` and `win()` function addresses. This writeup details the step-by-step process to solve the challenge.

---

#### Steps Taken:

1. **Binary Observation**:
   - The first step was to analyze the binary of the given program. We were instructed to enter an address, and from the binary analysis, we identified key function addresses.

2. **Identify `main()` and `win()` Addresses**:
   - The challenge asked us to figure out the address of the `win()` function. By observing the binary and understanding the program structure, we identified the memory address for the `main()` function.
   - The program provided us with the memory address of `main()` and instructed us to find the address of the `win()` function.

3. **Calculating the Offset**:
   - With the `main()` and `win()` addresses in hand, we converted these addresses from hexadecimal to decimal.
   - Next, we calculated the offset between the `main()` and `win()` addresses by subtracting the decimal value of the `win()` address from the decimal value of `main()`. This subtraction provided us with the offset, which remained consistent across attempts.

4. **Exploiting the Program Flow**:
   - Using the identified offset, we then jumped into the network communication (nc) program.
   - In the nc program, we followed the same procedure: finding the address of `main()`, converting it to decimal, and then subtracting the consistent offset to find the address of `win()`.

5. **Final Calculation**:
   - After obtaining the correct memory address of `win()`, we converted the new decimal address back to hexadecimal.
   - This address ultimately led us to the flag, which was located at the specific memory location.

6. **Flag Extraction**:
   - By following the above steps, we successfully derived the memory address that the program used to return the flag. The flag was located by referencing the computed address within the program’s memory.

---

#### Conclusion:
By analyzing the binary, calculating offsets, and manipulating addresses, we were able to exploit the program’s memory layout and obtain the flag for the "Pie Time" challenge. This required both binary analysis and memory manipulation skills, which are common in reverse engineering and CTF challenges.

