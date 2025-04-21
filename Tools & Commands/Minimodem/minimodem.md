
# Minimodem: Basic Guide for Audio-Based Data Transmission

**Minimodem** is a command-line utility that emulates a modem, enabling the transmission and reception of data over audio signals using modulation techniques such as Frequency Shift Keying (FSK). It is a powerful tool often utilized in CTF (Capture The Flag) challenges to decode hidden information within audio files.

---
## Installation
To install Minimodem on Debian-based systems:
```bash
sudo apt update
sudo apt install minimodem
```

---
## Transmitting Data
You can transmit text data by modulating it into an audio signal.
**Basic Syntax:**
```bash
minimodem --tx audiofile.wav < inputfile.txt
```

**Explanation:**
- `--tx`: Enables transmit mode.
- `audiofile.wav`: The output audio file containing modulated data.
- `< inputfile.txt`: Specifies the input text file to modulate.

You can also transmit directly through your speakers:
```bash
minimodem --tx < inputfile.txt
```

---
## Receiving Data
To demodulate and extract data from an audio signal:
```bash
minimodem --rx inputfile.wav > outputfile.txt
```

**Explanation:**
- `--rx`: Enables receive mode.
- `inputfile.wav`: The input audio file containing modulated data.
- `> outputfile.txt`: Saves the decoded output to a file.

---
## Common Options

|Option|Description|
|---|---|
|`--tx`|Transmit mode|
|`--rx`|Receive mode|
|`--rate [bps]`|Set the baud rate (default: 300)|
|`--freq [Hz]`|Set the carrier frequency (default: 1200 Hz)|
|`--fsk`|Use Frequency Shift Keying (default modulation technique)|

---
## Example Commands
**Transmit data at custom rate and frequency:**
```bash
minimodem --tx --rate 1200 --freq 1800 < inputfile.txt
```

**Receive data from a recorded audio file:**
```bash
minimodem --rx inputfile.wav > outputfile.txt
```

---
## CTF Use Case
Minimodem is a valuable tool in CTFs, particularly when dealing with challenges involving audio files. If a provided `.wav` or similar file contains beeping or unusual tones, it's often worth attempting to decode it using:
```bash
minimodem --rx challenge.wav
```

This can reveal hidden messages or flags encoded as audio signals.

---
## Recording & Playback Tools
To work effectively with Minimodem, you may also need tools for recording or playing audio:
- **Audacity**: A GUI-based audio editor for recording and inspecting waveforms.
- **SoX (Sound eXchange)**: A command-line audio processing tool.

**Record an audio signal using SoX:**
```bash
sox -d recording.wav
```

**Play a modulated audio signal through speakers:**
```bash
minimodem --tx < inputfile.txt
```

---
