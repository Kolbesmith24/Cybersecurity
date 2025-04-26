# Steganography Checklist

This document outlines essential techniques and tools for analyzing image files in CTF (Capture The Flag) challenges, particularly for uncovering hidden flags through steganographic methods.

---

## 1. Determine File Type

Before proceeding with further analysis, identify the actual file type:
```bash
type filename
```
This helps verify that the file extension is correct and gives insight into how to proceed.

---

## 2. Extract Readable Strings

Use the `strings` utility to identify potentially embedded readable text:
```bash
strings -n 7 -t x filename.png
```
- `-n 7`: Extracts strings of at least 7 characters.
- `-t x`: Displays the offset in hexadecimal.

Alternatively, online tools allow visualizing strings within an uploaded image.

---

## 3. Examine EXIF Metadata

Metadata can contain useful hints or embedded information. Use exiftool to see the metadata:
```
exiftool file.png
```

---

## 4. Use Binwalk for Embedded Files

Binwalk is powerful for identifying and extracting embedded files:
```bash
binwalk -Me filename.png
```
- `-M`: Enables recursive analysis of extracted files.
- `-e`: Automatically extracts embedded files.

Useful for discovering hidden archives, executables, or secondary image files.

---

## 5. Analyze PNG Structure with pngcheck

Use `pngcheck` to check for file integrity and hidden chunks:
```bash
pngcheck -vtp7f filename.png
```
- `-v`: Verbose mode.
- `-t` and `-7`: Show tEXt chunks (which may contain comments or metadata).
- `-p`: Displays contents of other optional chunks.
- `-f`: Forces checking beyond major errors (useful for corrupt files).

---

## 6. Explore Color and Bit Planes

Steganographic data is often hidden in specific color or bit planes:
- Upload to a site supporting bit plane analysis.
- Use tools on the top panel (e.g., Full Red, Inverse, LSB).
- Browse all bit planes under "Browse Bit Planes."
- Look for static or patterns suggesting hidden data.
- If found, use "Extract Files/Data" to recover contents.

---

## 7. Extract Data from LSB (Least Significant Bit)

If hidden content is suspected in bit planes:
- Navigate to "Extract Files/Data" on your bit plane analysis site.
- Select relevant planes (e.g., LSB of R, G, or B channels).

Useful for uncovering hidden text or files.

---

## 8. Analyze RGBA Values

Data can be hidden directly in the Red, Green, Blue, or Alpha (transparency) channels:
- Upload image to an RGBA value viewer.
- Inspect channel values for ASCII characters or readable strings.
- Try decoding individual R/G/B/A values separately.

---

## 9. Password-Protected Data & Steghide

If a password is found, or you suspect password-protected steganography, try:
```bash
steghide extract -sf filename.png
```
- It may not require a password if one wasn't used.

Other tools to try:
- OpenStego
- Stegpy
- Outguess
- jphide

---

## 10. Examine PNG Color Palettes

If the image is a PNG of type 3 (indexed color):
- Look through the color palette manually.
- Use tools that allow palette visualization and randomization.
- Sometimes flags are encoded via palette indices or color patterns.

---

## 11. Pixel Value Differencing (PVD / MPVD)

Rare but sophisticated steganographic method:
- Encodes data via differences between adjacent pixel values.
- Typically only used if explicitly mentioned in the challenge.


---

## Contribution

This checklist is a work in progress that has been expanded upon by me from https://georgeom.net/StegOnline/checklist


