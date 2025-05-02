# Quick Guide to `pdftotext` in Linux

`pdftotext` is a simple and powerful command-line tool to extract text from PDF files. It comes with the `poppler-utils` package and is super handy for converting PDFs into plain text for easier editing or processing.

---

## Basic Syntax

```bash
pdftotext [options] input.pdf [output.txt]
```

- If you don't specify an output file, it prints the text to the terminal.
- Output will be plain text, losing any images or formatting.

---

## Common Use Cases

### 1. Convert a PDF to a Text File
```bash
pdftotext file.pdf
```
- Saves the text into `file.txt` in the same directory.

---

### 2. Convert and Specify Output File Name
```bash
pdftotext file.pdf output.txt
```
- Saves the extracted text to `output.txt`.

---

### 3. Convert Specific Pages Only
```bash
pdftotext -f 2 -l 4 file.pdf output.txt
```
- `-f 2`: Start from page 2
- `-l 4`: End at page 4

---

### 4. Keep Original Layout (best effort)
```bash
pdftotext -layout file.pdf output.txt
```
- Tries to preserve the original text layout and spacing.

---

### 5. Convert PDF and Print to Terminal
```bash
pdftotext file.pdf -
```
- The `-` outputs the text directly to the terminal (STDOUT).

---

## Handy Options Summary

| Option     | Description                           |
|------------|---------------------------------------|
| `-layout`  | Preserve original layout and spacing  |
| `-f [n]`   | First page to extract                 |
| `-l [n]`   | Last page to extract                  |
| `-enc [e]` | Set text encoding (e.g., UTF-8)       |
| `-nopgbrk` | Don’t insert page breaks between pages|
| `-q`       | Suppress error messages               |
| `-`        | Output to terminal instead of file    |


## Learn More

```bash
man pdftotext
pdftotext -h
```
---
