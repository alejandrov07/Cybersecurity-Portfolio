# Linux Identity and Access Management: User Lifecycle and Group Authorization

## Executive Summary
This technical report documents the secure management of the user lifecycle within a Linux environment. The focus was on implementing the Principle of Least Privilege (PoLP) through precise group assignments, ownership transfer, and system hardening by removing orphaned accounts and deprecated groups.

## Technical Analysis
Effective Access Control (AC) is a cornerstone of system integrity. This exercise demonstrates the transition of a user through different organizational roles, requiring granular adjustments to their authorization levels.

### Key Operations Executed
* **Identity Provisioning:** Initialization of the `researcher9` identity and assignment to a specific primary functional group (`research_team`).
* **Resource Ownership Transfer:** Migration of file ownership for `project_r.txt` to the designated researcher using the `chown` utility, ensuring accountability.
* **Privilege Expansion:** Utilizing `usermod -aG` to grant supplementary access to the `sales_team` without compromising the user's primary departmental affiliation.
* **Deprovisioning & Cleanup:** Full removal of the user identity and its associated unique group to prevent "shadow" accounts or orphaned GIDs (Group Identifiers).

## Security Insight
From a Blue Team perspective, inadequate user lifecycle management leads to **Privilege Creep**. This occurs when employees retain access to legacy data after changing roles. Regular auditing of `/etc/group` and `/etc/passwd` is mandatory to ensure that users do not possess lateral movement capabilities across sensitive directories. Furthermore, deleting the user's private group upon offboarding is a critical step in system hygiene to maintain a clean security descriptors table and prevent GID collision in future provisioning.

## Detailed Methodology
1.  **User Creation:** `sudo useradd researcher9`
    `sudo usermod -g research_team researcher9`
2.  **Access Control Adjustment:**
    `sudo chown researcher9 /home/researcher2/projects/project_r.txt`
3.  **Cross-Departmental Authorization:**
    `sudo usermod -a -G sales_team researcher9`
4.  **Secure Offboarding:**
    `sudo userdel researcher9`
    `sudo groupdel researcher9`

*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in Linux Identity and Access Management (IAM) and system security hardening.*
