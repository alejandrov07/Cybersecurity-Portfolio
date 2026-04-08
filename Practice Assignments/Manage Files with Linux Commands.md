# 🐧 Manage Files with Linux Commands

**Course:** Google Cybersecurity Certificate  
**Type:** Hands-on Lab — Linux File Management  
**Tool Used:** Linux Bash Shell, nano  
**Environment:** Debian-based Linux — Bash Shell  

---

## 📋 Scenario

As a security analyst, keeping data well organized makes it easier to detect issues and maintain data security. In this lab, I reorganized the `/home/analyst` directory by creating and removing directories, moving and deleting files, and documenting the changes using the nano text editor.

**Initial structure:**
```
home
└── analyst
    ├── notes
    │   ├── Q3patches.txt
    │   └── tempnotes.txt
    ├── reports
    │   ├── Q1patches.txt
    │   └── Q2patches.txt
    └── temp
```

**Target structure:**
```
home
└── analyst
    ├── logs
    ├── notes
    │   └── tasks.txt
    └── reports
        ├── Q1patches.txt
        ├── Q2patches.txt
        └── Q3patches.txt
```

---

## 🧰 Tools & Methods

- **mkdir** — Creates a new directory
- **rmdir** — Removes an empty directory
- **mv** — Moves a file to a different directory
- **rm** — Removes a file
- **touch** — Creates a new empty file
- **nano** — Text editor used to add content to a file
- **ls** — Lists directory contents to verify each change
- **cat** — Displays file contents

---

## 📄 Tasks Completed

### Task 1 — Create a New Directory

Created a new `logs` subdirectory inside `/home/analyst` to store future log files.

```bash
mkdir logs
ls
```

**Output:**
```
logs  notes  reports  temp
```

---

### Task 2 — Remove a Directory

Removed the `temp` directory as it was no longer needed.

```bash
rmdir temp
ls
```

**Output:**
```
logs  notes  reports
```

---

### Task 3 — Move a File

Navigated to the `notes` directory and moved `Q3patches.txt` to the `reports` directory, where it belonged.

```bash
cd /home/analyst/notes
mv Q3patches.txt /home/analyst/reports/
ls /home/analyst/reports
```

**Output:**
```
Q1patches.txt  Q2patches.txt  Q3patches.txt
```

---

### Task 4 — Remove a File

Deleted the unused `tempnotes.txt` file from the `notes` directory.

```bash
rm tempnotes.txt
ls
```

**Output:**
```
(empty — no files remaining in notes)
```

---

### Task 5 — Create a New File

Created an empty `tasks.txt` file in the `notes` directory to document completed tasks.

```bash
touch tasks.txt
ls
```

**Output:**
```
tasks.txt
```

---

### Task 6 — Edit a File

Opened `tasks.txt` with the nano text editor and added a note documenting the completed tasks.

```bash
nano tasks.txt
```

**Content added:**
```
Completed tasks
1. Managed file structure in /home/analyst
```

Saved and exited nano, then verified the file contents:

```bash
cat tasks.txt
```

**Output:**
```
Completed tasks
1. Managed file structure in /home/analyst
```

---

## 📝 Commands Reference

| Command | Description |
|---------|-------------|
| `mkdir <dir>` | Creates a new directory |
| `rmdir <dir>` | Removes an empty directory |
| `mv <file> <destination>` | Moves a file to a specified destination |
| `rm <file>` | Removes a file |
| `touch <file>` | Creates a new empty file |
| `nano <file>` | Opens a file in the nano text editor |
| `cat <file>` | Displays the full contents of a file |
| `ls` | Lists the contents of the current directory |
| `clear` | Clears the Bash shell window |

---

*This project is part of my cybersecurity portfolio, completed as a hands-on lab in Linux file and directory management.*
