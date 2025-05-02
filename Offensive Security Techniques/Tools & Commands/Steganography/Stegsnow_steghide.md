# Quick Guide to Stegsnow & Steghide in Linux

**Stegsnow** and **Steghide** are steganography tools used to hide secret messages inside files like text or images.  
They're often used in Capture The Flag (CTF) challenges, penetration testing, or privacy-related tasks.

---

## Stegsnow (Whitespace Steganography)

**Stegsnow** hides messages in the **whitespace (spaces/tabs)** at the end of lines in text files. Invisible to the eye!

### Install
```bash
sudo apt install stegsnow
```

---

### Hide a Message
```bash
stegsnow -C -p secretpassword cover.txt stego.txt
```
- `-C`: Compress the message before hiding
- `-p`: Use a password for encryption
- `cover.txt`: The normal-looking text file
- `stego.txt`: The output with hidden data

---

### Reveal a Hidden Message
```bash
stegsnow -p secretpassword stego.txt
```
- Reveals the hidden message using the password.
- No password needed if none was used while hiding.

---

## Stegsnow Quick Tips

| Option     | Description                            |
|------------|----------------------------------------|
| `-C`       | Compress message before hiding         |
| `-p`       | Use password for encryption            |
| `cover.txt`| Input cover file (text)                |
| `stego.txt`| Output file with hidden data           |

---

# Steghide (File Steganography)

**Steghide** hides data inside **images (JPG, BMP)** or **audio files (WAV, AU)**.

### Install
```bash
sudo apt install steghide
```

---

### Embed a File into an Image
```bash
steghide embed -cf image.jpg -ef secret.txt
```
- `-cf`: Cover file (e.g. image.jpg)
- `-ef`: Embedded file (e.g. secret.txt)
- It’ll ask you for a passphrase.

---

### Extract a Hidden File
```bash
steghide extract -sf image.jpg
```
- `-sf`: Stego file (image containing hidden data)
- It’ll prompt for the passphrase used during embedding.

---

### Check Stego File Info
```bash
steghide info image.jpg
```
- Displays info about whether the file contains embedded data.

---

## Steghide Quick Tips

| Option       | Description                             |
|--------------|-----------------------------------------|
| `embed`      | Command to embed data                   |
| `extract`    | Extract hidden data                     |
| `-cf`        | Cover file (image/audio)                |
| `-ef`        | File to embed                           |
| `-sf`        | Stego file (with data)                  |
| `info`       | Show info about potential hidden data   |

---

## Good to Know

- **Stegsnow** works best with `.txt` files only.
- **Steghide** supports only specific formats (JPG, BMP, WAV, AU).
- Passwords are optional but add an extra layer of protection.

---

## Learn More

```bash
man steghide
man stegsnow
```
---
