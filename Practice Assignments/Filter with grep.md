# 🐧 Filter with grep

**Course:** Google Cybersecurity Certificate  
**Type:** Hands-on Lab — Linux File Filtering  
**Tool Used:** Linux Bash Shell  
**Environment:** Debian-based Linux — Bash Shell  

---

## 📋 Scenario

As a security analyst, efficiently locating specific information within files and directories is a critical skill. In this lab, I used the `grep` command and piping to search server logs for error messages, find files containing specific strings in their names, and extract user data from report files.

---

## 🧰 Tools & Methods

- **grep** — Searches for specific strings within files or command output
- **ls** — Lists the contents of a directory
- **cd** — Changes the current working directory
- **pipe (`|`)** — Sends the output of one command as input to another for further filtering

---

## 📄 Tasks Completed

### Task 1 — Search for Error Messages in a Log File

Navigated to the `/home/analyst/logs` directory and used `grep` to filter `server_logs.txt` for all lines containing the string `error`.

```bash
cd /home/analyst/logs
grep error server_logs.txt
```

**Key findings:**
- The `server_logs.txt` file contained **6 error lines**

---

### Task 2 — Find Files Containing Specific Strings

Navigated to the `/home/analyst/reports/users` directory and used piping to filter the output of `ls` with `grep`, searching for files containing specific strings in their names.

**Search for files containing "Q1":**
```bash
cd /home/analyst/reports/users
ls | grep Q1
```

**Key findings:**
- **3 files** in the directory contained `Q1` in their names

**Search for files containing "access":**
```bash
ls | grep access
```

**Key findings:**
- **4 files** in the directory contained `access` in their names

---

### Task 3 — Search File Contents for User Data

Searched specific user report files to identify users that were added and deleted from the system.

**Search for username `jhill` in the deleted users file:**
```bash
grep jhill Q2_deleted_users.txt
```

**Key findings:**
- Username `jhill` **was found** in the `Q2_deleted_users.txt` file, confirming they were deleted from the system in Q2

**Search for users added to the Human Resources department in Q4:**
```bash
grep "Human Resources" Q4_added_users.txt
```

**Key findings:**
- **2 users** were added to the Human Resources department in Q4

---

## 📝 Commands Reference

| Command | Description |
|---------|-------------|
| `grep <string> <file>` | Searches for a string within a file and returns all matching lines |
| `ls \| grep <string>` | Filters the output of `ls` to show only files matching the string |
| `grep "<multi-word string>" <file>` | Searches for a multi-word string — quotes are required |
| `cd <path>` | Navigates to the specified directory |

---

*This project is part of my cybersecurity portfolio, completed as a hands-on lab in Linux file filtering and log analysis using grep.*
