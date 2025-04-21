# Chunked Integrity – DawgCTF 2025
## Reconnaissance
The challenge provided a PNG file for analysis, saying that something happened to the image and now they full image is not being displayed. As a first step, I downloaded the image using `wget` to begin inspecting it locally:

![image](https://github.com/user-attachments/assets/826e877e-13b7-4781-b062-468cbcfbf2a2)

![image](https://github.com/user-attachments/assets/177a18f5-af52-4c12-a6f1-cbbbb07d50c2)

Here we can see the PNG, which we can't see the bottom portion as it may be corrupt:

![image](https://github.com/user-attachments/assets/14cd1a4f-42e9-4e5f-b5dd-55774c6cd614)

---
## File Analysis
To validate the integrity of the PNG, I used `pngcheck`, a utility designed to inspect and report errors in PNG files:
```
pngcheck corrupted.png
```
![image](https://github.com/user-attachments/assets/94a6ac8d-a8ff-4a13-977f-854e1888a948)

The output revealed that the PNG file was malformed due to corrupted or missing chunks—an indication that the file structure had been tampered with, likely as part of the challenge’s obfuscation.

---
## File Repair with Chunklate
To recover the contents of the image, I used a specialized tool called [Chunklate](https://github.com/on4r4p/Chunklate), which is designed to identify and reconstruct corrupted PNG chunks:
```
python3 chunklate.py -f funnyCat.png
```
![image](https://github.com/user-attachments/assets/cc3f0cfd-92fe-4c02-8ba4-9ec12a341b8b)

![image](https://github.com/user-attachments/assets/cd17a5c1-402a-4c30-8987-b37d1eb517e5)

Chunklate successfully repaired the file structure, allowing it to be rendered and viewed properly.

---
## Flag Discovery
After opening the repaired image (`repaired.png`), the hidden flag became visible within the image content.

![image](https://github.com/user-attachments/assets/d38e7a8f-0bab-4bf7-91a4-af320e05dfe9)
