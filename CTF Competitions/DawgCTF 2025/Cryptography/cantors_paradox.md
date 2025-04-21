# Cantor's Paradox - DawgCTF 2025
## Reconnaissance
Upon inspecting the challenge, two files are provided:
- `chall.py`: A Python script that obfuscates a flag using a custom pairing function inspired by Cantor's pairing technique. This function is applied **recursively**, ultimately producing a single integer that represents the original flag.
- `output.txt`: A file containing the final obfuscated output, which is a single large integer.

![image](https://github.com/user-attachments/assets/62690f41-6109-4236-8f74-2f0bae9f2523)

The objective is to reverse this process and retrieve the original flag.

---
## Code Analysis
By analyzing `chall.py`, we observe the following key logic:
1. The flag is first converted into ASCII values.
2. These values are recursively paired using a custom `pair()` function.
3. A depth of 6 is used for recursion, meaning the process continues until all values are reduced to a single number.
4. Padding is added if the length isn't suitable for even pairing.
5. A `getTriNumber(n)` function is used internally, which we’ll need to reverse as well.

---
## Strategy for Solving
To retrieve the original flag, we need to reverse the entire obfuscation process step by step. This includes:
- **Reading the final integer** from `output.txt`.
- **Reversing `getTriNumber(n)`**, which involves solving for the triangular root of a number.
- **Reversing the `pair(n1, n2)`** function to retrieve the two original numbers (`unpair`).
- **Recursively unpairing** the number until we reach the base ASCII values (depth of 6).
- **Removing padding**, if any was added during the pairing phase.
- **Converting the ASCII values back to characters** to retrieve the flag.

---
## Solution Script
The reverse process looks something like this:

![image](https://github.com/user-attachments/assets/0ee95b4f-0c34-485c-a553-b15693560fef)


---
## Final Output
Running the script successfully recovers the original flag:

![image](https://github.com/user-attachments/assets/924f1f20-2637-4744-9885-b1b844dad6e4)
