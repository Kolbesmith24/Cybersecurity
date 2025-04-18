# Quick Guide to Sleuth Kit & Autopsy in Linux

**Sleuth Kit (TSK)** is a powerful set of command-line tools for digital forensics, used to analyze disk images and recover evidence.  
**Autopsy** is a GUI built on top of Sleuth Kit that makes forensic analysis easier and more visual.

---

## Sleuth Kit: Command-Line Tools

Sleuth Kit includes tools for examining disk images, file systems, and recovering deleted data.

---

### 1. View Partition Info (`mmls`)
```bash
mmls disk.img
```
- Shows partition layout of a disk image.
- Use this to find the start offset of partitions.

---

### 2. List Files in a Partition (`fls`)
```bash
fls -o 2048 disk.img
```
- Lists files and directories in a file system.
- `-o 2048` sets the partition offset (from `mmls`).

---

### 3. Extract a File (`icat`)
```bash
icat -o 2048 disk.img 5 > recovered.jpg
```
- Recovers file with ID 5 from the disk image.
- Save output using `>` to create a new file.

---

### 4. Show File System Info (`fsstat`)
```bash
fsstat -o 2048 disk.img
```
- Displays metadata and details of the file system.

---

### 5. Search for Deleted Files (`tsk_recover`)
```bash
tsk_recover -o 2048 disk.img output_folder/
```
- Recovers all files (including deleted ones) into a folder.

---

### 6. Timeline Creation (`mactime`)
```bash
fls -m / -r -o 2048 disk.img > bodyfile.txt
mactime -b bodyfile.txt > timeline.csv
```
- Builds a timeline of file activity (Modified, Accessed, Changed, Created).
- Helpful in tracking user or attacker behavior over time.

---

## Sleuth Kit Tool Summary

| Tool       | Description                           |
|------------|---------------------------------------|
| `mmls`     | Show partition layout                 |
| `fls`      | List file and directory metadata      |
| `icat`     | Extract file contents by inode        |
| `fsstat`   | File system stats                     |
| `tsk_recover` | Recover all files to a directory |
| `mactime`  | Create a timeline from file metadata  |

---

## Autopsy: GUI for Forensics

Autopsy is a graphical front-end that uses Sleuth Kit under the hood. It’s easier to use for beginners and provides:

- File browsing with a visual tree
- Keyword search
- Timeline analysis
- Web history and email recovery
- EXIF and metadata extraction
- Hash matching and hash set creation

---

### 🔧 Starting Autopsy

After installation:

```bash
autopsy
```

- Open a browser and follow the link (usually http://localhost:9999).
- Create a case, add data source (like a disk image), and start analyzing.

---

## Installation

### Sleuth Kit
```bash
sudo apt install sleuthkit
```

### Autopsy (Manual Installation Recommended)
Autopsy isn’t always in the default Linux repos, so install from source or use a virtual machine image.

Download from: https://www.sleuthkit.org/autopsy/

---

##  Learn More

```bash
man mmls
man fls
man icat
```

Or visit: https://www.sleuthkit.org

---

