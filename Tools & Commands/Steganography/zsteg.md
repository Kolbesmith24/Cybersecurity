# Quick Guide to `zsteg` in Linux

**zsteg** is a Ruby-based steganography tool used to detect and extract hidden data from images (especially **PNG** and **BMP** formats).  
It’s commonly used in CTFs and digital forensics to analyze image files for hidden messages.

---

## Installation

### Prerequisites
Make sure you have Ruby and bundler:
```bash
sudo apt install ruby ruby-dev
```

### Install zsteg
```bash
sudo gem install zsteg
```

---

## Basic Usage

### 1. Scan a File for Hidden Data
```bash
zsteg file.png
```
- Runs default detection on all common LSB and color channels.
- Looks for ASCII, UTF-8, UTF-16, and other hidden strings.

---

### 2. Use Verbose Output
```bash
zsteg -a file.png
```
- `-a`: Try **all** known detection methods and formats.
- Useful for thorough scans.

---

### 3. Check Specific Channel and Format
```bash
zsteg -E b1,rgb,lsb,xy file.png
```
- `-E`: Extract using a specific method.
- Example above: Extract 1-bit LSB from RGB channels in XY order.

---

### 4. Extract as Raw Bytes
```bash
zsteg -E b1,r,lsb,xy file.png > output.bin
```
- Saves the raw hidden data to `output.bin`.

---

### 5. Show Only Matching Results
```bash
zsteg -s file.png
```
- Only shows output if something meaningful is detected (silent on empty results).

---

## zsteg Option Summary

| Option       | Description                                    |
|--------------|------------------------------------------------|
| `-a`         | Try all detection methods                      |
| `-s`         | Silent mode, show only hits                    |
| `-E method`  | Extract using specified method (e.g. `b1,r`)   |
| `--strings`  | Show printable strings (like `strings` command)|
| `--lsb`      | Use only LSB channels                          |

---

## Tips

- Works best on **uncompressed** image formats (BMP, PNG).
- Doesn't work on JPG (lossy compression destroys hidden data).
- Pair with tools like `binwalk`, `steghide`, or `strings` for deeper analysis.

---

## Learn More

```bash
zsteg --help
```

Or check the GitHub repo: https://github.com/zed-0xff/zsteg

