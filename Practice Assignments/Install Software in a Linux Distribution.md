# 🐧 Install Software in a Linux Distribution

**Course:** Google Cybersecurity Certificate  
**Type:** Hands-on Lab — Linux Package Management  
**Tool Used:** APT (Advanced Package Tool)  
**Environment:** Debian-based Linux — Bash Shell  

---

## 📋 Scenario

As a security analyst, I was required to have the Suricata and tcpdump network security applications installed on my Linux system. In this lab, I installed, uninstalled, and reinstalled these applications using the APT package manager in a Linux Bash shell, and confirmed correct installation at each step.

---

## 🧰 Tools & Methods

- **APT (Advanced Package Tool)** — Package manager used to install, remove, and list applications on Debian-based Linux systems
- **sudo** — Used to execute commands with elevated privileges, required for installing and uninstalling software
- **Bash Shell** — Command-line interface used to run all commands throughout the lab

---

## 📄 Tasks Completed

### Task 1 — Confirm APT is Installed

Verified that APT was available in the Linux environment by running the `apt` command and confirming that version and usage information was returned.

```bash
apt
```

**Output:**
```
apt 1.8.2.3 (amd64)
Usage: apt [options] command
...
```

APT is installed by default on Debian-based systems and is the recommended package manager for this distribution.

---

### Task 2 — Install and Uninstall Suricata

Installed Suricata, a network analysis tool used for intrusion detection, using APT with elevated privileges.

```bash
sudo apt install suricata
```

Verified the installation by running the application:

```bash
suricata
```

**Output:**
```
Suricata 4.1.2
USAGE: suricata [OPTIONS] [BPF FILTER]
...
```

Then uninstalled Suricata and confirmed removal:

```bash
sudo apt remove suricata
suricata
```

**Output after removal:**
```
-bash: /usr/bin/suricata: No such file or directory
```

This confirmed that Suricata was successfully uninstalled.

---

### Task 3 — Install tcpdump

Installed tcpdump, a command-line tool used to capture and analyze network traffic in a Linux Bash shell.

```bash
sudo apt install tcpdump
```

---

### Task 4 — List Installed Applications

Used APT to list all installed applications and confirmed that tcpdump was present. Suricata was not listed, as it had been uninstalled in the previous task.

```bash
apt list --installed
```

**Relevant output:**
```
tcpdump/oldstable,now 4.9.3-1~deb10u2 amd64 [installed]
```

---

### Task 5 — Reinstall Suricata

Reinstalled Suricata and confirmed that both Suricata and tcpdump were listed as installed applications.

```bash
sudo apt install suricata
apt list --installed
```

**Relevant output:**
```
suricata/oldstable,now 1:4.1.2-2+deb10u1 amd64 [installed]
tcpdump/oldstable,now 4.9.3-1~deb10u2 amd64 [installed]
```

Both applications were confirmed as successfully installed.

---

## 📝 Commands Reference

| Command | Description |
|---------|-------------|
| `apt` | Runs the APT package manager and displays usage information |
| `sudo apt install <app>` | Installs an application with elevated privileges |
| `sudo apt remove <app>` | Uninstalls an application with elevated privileges |
| `apt list --installed` | Lists all currently installed applications |

---

*This project is part of my cybersecurity portfolio, completed as a hands-on lab in Linux system administration and package management.*
