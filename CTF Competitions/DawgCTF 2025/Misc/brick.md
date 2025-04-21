# Brick - DawgCTF 2025
## Challenge Overview
In this challenge, we’re given a `.wav` file and told it originated from an old tape from the 1980s. The goal is to extract a hidden flag from the audio, suggesting a vintage computing or data-encoding angle.

---
## Reconnaissance Phase
The file format and context provide the first clues:
- We're handed a `.wav` file—an uncompressed audio format.
- The description references the 1980s and tapes, indicating the challenge might involve retro computing, such as cassette-based data storage common on devices like the Commodore 64.
- Knowing that audio tapes were used to store executable data on such systems, it's likely the `.wav` file contains a program that needs to be converted and executed.

---
## File Conversion and Analysis
The first step is to convert the `.wav` file into a format the Commodore 64 can understand.
1. **Convert `.wav` to `.tap` (Tape Image Format)**  
    Since `wav2prg` is deprecated, we used **AudioTap**, a Windows-only tool, and ran it via `wine` on Linux:
    ```bash
    sudo apt install wine wine32 wine64 libwine libwine:i386 fonts-wine
    winecfg
    wine audiotap.exe
    ```
    - AudioTap allows us to convert audio representations of data into `.tap` files.
2. **Convert `.tap` to `.prg` (Program File Format)** 
    With the `.tap` file ready, we use `cbmconvert`, a tool designed to convert Commodore file formats:
    ```bash
    sudo apt install cbmconvert
    cbmconvert myfile.tap
    ```
    This outputs a `.prg` file, the format used for executable programs on the Commodore 64.

---
## Execution via Emulator
To run the `.prg` file, we need a Commodore 64 emulator. I used **VICE (Versatile Commodore Emulator)**. Since the Linux version is no longer actively maintained, I ran it on a Windows machine:
1. Transfer the `.prg` file to the Windows system.
2. Open the C64 emulator via VICE.
3. Load and execute the `.prg` file.

Upon execution, the program displays the hidden flag:

![image](https://github.com/user-attachments/assets/7ac62e0f-7ef2-4259-b279-6d8c84f54ffa)
