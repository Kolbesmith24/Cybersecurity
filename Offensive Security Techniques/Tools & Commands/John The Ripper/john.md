# Quick Guide to John the Ripper in Linux

**John the Ripper** (often called `john`) is a fast password-cracking tool used to test password strength by performing dictionary, brute-force, and hybrid attacks on hashed password files.

---

## Installation

### From Package Manager (basic version):
```bash
sudo apt install john
```

### From Source (for full Jumbo version):
```bash
git clone https://github.com/openwall/john.git
cd john/src
./configure && make -s clean && make -sj4
```

---

## Basic Workflow

1. **Obtain password hash file** (e.g., `/etc/shadow`, zip hash, etc.).
2. **Prepare a wordlist** (like `rockyou.txt`).
3. **Crack the hashes** using `john`.

---

## Common Commands

### 1. Crack Hashes with a Wordlist
```bash
john --wordlist=rockyou.txt hashes.txt
```
- Tries passwords from `rockyou.txt` against the hashes in `hashes.txt`.

---

### 2. Show Cracked Passwords
```bash
john --show hashes.txt
```
- Displays any passwords that have already been cracked.

---

### 3. Identify Hash Format (Optional)
```bash
john --list=formats
```
- Lists supported hash formats (e.g., MD5, SHA512, bcrypt, etc.).

---

### 4. Crack Zip/RAR File (Using hash file)
First, extract the hash using a tool like `zip2john`:
```bash
zip2john secret.zip > ziphash.txt
john --wordlist=rockyou.txt ziphash.txt
```

---

### 5. Crack Linux Shadow Hashes
```bash
unshadow passwd.txt shadow.txt > fullhashes.txt
john --wordlist=rockyou.txt fullhashes.txt
```
- `unshadow` merges `/etc/passwd` and `/etc/shadow`.

---

## Helpful Utilities

| Tool         | Purpose                               |
|--------------|----------------------------------------|
| `unshadow`   | Merge passwd + shadow for cracking     |
| `zip2john`   | Extract hashes from `.zip` files       |
| `rar2john`   | Extract hashes from `.rar` files       |
| `pdf2john`   | Extract hashes from `.pdf` files       |
| `john`       | Main cracking engine                   |

---

## Tips

- Use `--format=formatname` if John can't detect the hash automatically.
- Keep your wordlist strong and varied (e.g., `rockyou.txt`, `SecLists`).
- Cracking speed varies based on hash strength and system performance.

---

## Learn More

```bash
man john
john --help
```

Official site: https://www.openwall.com/john/

GitHub (Jumbo): https://github.com/openwall/john

---
