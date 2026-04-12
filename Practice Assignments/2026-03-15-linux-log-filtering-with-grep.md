# Linux File Filtering and Log Analysis Using Grep

## Overview
This project demonstrates practical log analysis and file filtering techniques using standard Linux command-line tools. The objective was to efficiently locate security-relevant information across directories and files, simulating common tasks performed by a security analyst during incident investigation and user activity reviews.

The work was conducted in a Debian-based Linux environment using the Bash shell, focusing on the effective use of `grep`, directory navigation, and command piping.

## Environment
- Operating System: Debian-based Linux
- Shell: Bash
- Analysis Context: Server logs and user report files

## Techniques Applied

### Filtering Log Files for Error Events
Server logs were analyzed to identify error-related events by searching for specific keywords within log files.

```bash
cd /home/analyst/logs
grep error server_logs.txt
