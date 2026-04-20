# Enforcing Least Privilege Through Linux File and Directory Permissions

## Overview
This project focused on auditing and correcting Linux file and directory permissions within a user-owned workspace to enforce the principle of least privilege. The objective was to identify excessive access granted to users, groups, or others and remediate those misconfigurations to reduce the attack surface and prevent unauthorized access to sensitive research files.

The environment simulated a multi-user system where improper permissions could lead to data exposure or lateral access by unintended users.

## Environment
- Operating System: Linux
- Shell: Bash
- User: researcher2
- Group: research_team
- Working Directory: `/home/researcher2/projects`

## Actions Performed
- Audited permissions on all files and directories, including hidden files.
- Identified files granting write permissions to `other`, violating security policy.
- Removed unauthorized write access from publicly accessible files.
- Restricted group access on sensitive files intended for user-only access.
- Corrected permissions on archived hidden files to enforce read-only access.
- Hardened directory permissions by removing group execute access to prevent traversal.

## Key Commands Used
```bash
ls -la
chmod o-w project_k.txt
chmod g-rw project_m.txt
chmod u-w,g-w .project_x.txt
chmod g-x drafts
``
