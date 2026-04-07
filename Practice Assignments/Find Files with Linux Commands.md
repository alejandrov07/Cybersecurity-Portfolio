# 🐧 Find Files with Linux Commands

**Course:** Google Cybersecurity Certificate  
**Type:** Hands-on Lab — Linux File System Navigation  
**Tool Used:** Linux Bash Shell  
**Environment:** Debian-based Linux — Bash Shell  

---

## 📋 Scenario

As a security analyst, navigating a Linux file system without a graphical user interface is an essential skill — for example, when investigating unauthorized access by reading user access reports remotely. In this lab, I navigated the `/home/analyst` directory structure, located specific files, and read their contents using core Linux Bash commands.

---

## 🧰 Tools & Methods

- **pwd** — Displays the current working directory
- **ls** — Lists the contents of a directory
- **cd** — Changes the current working directory
- **cat** — Displays the full contents of a file
- **head** — Displays the first 10 lines of a file

---

## 📄 Tasks Completed

### Task 1 — Get Current Directory Information

Displayed the current working directory and listed its contents.

```bash
pwd
ls
```

**Output:**
```
/home/analyst

logs  reports  temp  tools
```

The current working directory was `/home/analyst`, which contained four subdirectories.

---

### Task 2 — Change Directory and List Subdirectories

Navigated to the `/home/analyst/reports` directory and listed its contents to identify the subdirectories it contained.

```bash
cd /home/analyst/reports
ls
```

**Output:**
```
users
```

The `reports` directory contained one subdirectory: `users`.

---

### Task 3 — Locate and Read the Contents of a File

Navigated to the `users` subdirectory, listed its files, and displayed the contents of the `Q1_added_users.txt` file to analyze user account information.

```bash
cd /home/analyst/reports/users
ls
cat Q1_added_users.txt
```

**Key findings from the file:**
- The employee with username `aezra` works in the **Human Resources** department
- The employee `mreed` in the **Information Technology** department has employee ID **1104**

---

### Task 4 — Navigate to a Directory and Locate a File

Navigated to the `/home/analyst/logs` directory, identified the file it contained, and displayed the first 10 lines of the server log file to analyze its contents.

```bash
cd /home/analyst/logs
ls
head server_logs.txt
```

**Key findings:**
- The logs directory contained the file `server_logs.txt`
- The first 10 lines of the file contained **three warning messages**

---

## 📝 Commands Reference

| Command | Description |
|---------|-------------|
| `pwd` | Displays the current working directory path |
| `ls` | Lists files and directories in the current directory |
| `cd <path>` | Navigates to the specified directory |
| `cat <file>` | Displays the full contents of a file |
| `head <file>` | Displays the first 10 lines of a file |

---

*This project is part of my cybersecurity portfolio, completed as a hands-on lab in Linux file system navigation and command-line analysis.*
