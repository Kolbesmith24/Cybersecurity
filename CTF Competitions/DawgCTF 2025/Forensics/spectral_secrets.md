# Spectral Secrets - DawgCTF 2025
## Initial Analysis:
After downloading the file, I identified it as a WAVE audio file. My first step was to attempt common steganography tools such as `steghide`, even trying extraction without a passphrase, but this yielded no results.

![image](https://github.com/user-attachments/assets/8c34cab1-1820-421d-832c-a2cc4db056e7)


---
## Metadata and File Inspection:
I then examined the file's metadata and ran `binwalk` to check for any embedded files, hidden executables, or unusual data segments. However, no meaningful output was found through this method.

![image](https://github.com/user-attachments/assets/11503b7f-fb3f-4456-826a-1940c42f6d68)

---
## Auditory Inspection:
Next, I listened to the audio file. It consisted of repetitive robotic-sounding tones, reminiscent of buttons being pressed in a pattern. To explore the possibility of audio manipulation, I attempted reversing the audio and slowing it down. Still, no intelligible message or flag was found through direct listening.

![image](https://github.com/user-attachments/assets/005156af-20e2-45ec-8b83-15ab654b46d1)

---
## Spectrogram Analysis:
Finally, I visualized the audio using a spectrogram. This revealed that the flag had been hidden visually within the frequency domain of the sound. The robotic noises were encoding characters that could only be seen when transformed into a visual representation of the frequency spectrum.

![image](https://github.com/user-attachments/assets/42c7e50a-289d-427c-a70d-fc539b8bcc61)
