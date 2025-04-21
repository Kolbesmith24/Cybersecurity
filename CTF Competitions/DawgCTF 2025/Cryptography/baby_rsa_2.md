## Baby RSA 2 - DawgCTF 2025
### Reconnaissance
The challenge provided two files:
- `chall.py` – containing the encryption logic and RSA parameters.
- `output.txt` – containing the RSA public exponent `e`, private exponent `d`, modulus `N`, and the ciphertext `c`.
![image](https://github.com/user-attachments/assets/f45e3f1b-3bbd-46bb-bef2-34f5090ed487)

From the provided information, we know this is an RSA decryption challenge. The interesting twist is that while we are given the private exponent `d`, we are not given the prime factors `p` and `q` of `N`, which are typically needed to derive the totient φ(N).

Our objective is to recover the plaintext flag from the ciphertext `c` using a cryptographic trick that allows us to recover `p` and `q` from the relationship between `e`, `d`, and φ(N).
### Vulnerability & Exploit Strategy
RSA encryption and decryption rely on the mathematical relationship:
```
e * d ≡ 1 mod φ(N)
```

This implies that:
```
e*d - 1 is divisible by φ(N)
```

Knowing this, we can attempt a brute-force search to recover φ(N). For each candidate value of `k`, we check if:
```
φ(N) = (e * d - 1) / k
```

We then use the known identity:
```
N = p * q
φ(N) = (p - 1)(q - 1)
=> p + q = N - φ(N) + 1
```

Using this, we treat `p` and `q` as the roots of a quadratic equation and solve for them. If the derived values of `p` and `q` multiply to `N`, we have successfully factored the modulus.
### Decryption Process
Once we’ve obtained `p` and `q`, we recalculate φ(N) using:
```
φ(N) = (p - 1)(q - 1)
```

We then compute the correct private key `d` with:
```python
d = inverse(e, φ(N))
```

With `d` and `N`, we can decrypt the ciphertext `c` using Python’s built-in `pow()` function and convert the result to bytes to reveal the flag.
### Final Python Script
![image](https://github.com/user-attachments/assets/6700ce57-5590-4170-a94f-91997bcae20d)

### Output
Running the script successfully reveals the flag:
![image](https://github.com/user-attachments/assets/800a642b-6ba1-4b36-86a0-4f8b02641f46)
