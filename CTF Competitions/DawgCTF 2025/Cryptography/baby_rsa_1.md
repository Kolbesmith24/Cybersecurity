# Baby RSA 1 - DawgCTF 2025
## Reconnaissance
The challenge provides us with a **Python script** and an **output file** containing the following RSA-related values:
- `N` (modulus)
- `e` (public exponent)
- `ct` (ciphertext)
- `x` and `y`, two linear combinations of the unknown primes `p` and `q`

From analyzing the code and the output, it is revealed that:
```
x = a*p + b*q  
y = c*p + d*q
```

The goal is to determine the prime factors `p` and `q` of the modulus `N` using the above equations, and then decrypt the ciphertext `ct` using RSA decryption.

---

## Environment Setup
The provided script required specific Python packages, so I created a **virtual environment** to run it cleanly:
1. Create a virtual environment
```bash
python3 -m venv ~/myenv
```
2. Activate the environment
```
source ~/myenv/bin/activate
```
3. Install dependencies
```
pip install pycryptodome sympy
```

---

## Exploitation & Decryption
Once the environment was prepared, I ran the RSA decryption script (`rsa_decrypt.py`), which performed the following steps:
1. **Solving for `p` and `q`**  
    The script uses `sympy` to solve a system of linear equations to recover the RSA primes `p` and `q` using the values of `x` and `y`.
2. **Computing Private Key**  
    With `p` and `q` known, it computes φ(N) = (p - 1)(q - 1), and then calculates the modular inverse of `e` mod φ(N) to obtain the private exponent `d`.
3. **Decrypting the Ciphertext**  
    Using `d`, the script decrypts the ciphertext
4. Finally, it converts the plaintext integer into bytes to reveal the flag.
   
![image](https://github.com/user-attachments/assets/4547fab8-bd1f-4c2f-8937-e48232168e17)

---

## Flag Recovered
Upon running our script, we successfully print out the flag:

![image](https://github.com/user-attachments/assets/8fa7cee8-794c-4f3d-a0b2-e7ec4a86ffec)

