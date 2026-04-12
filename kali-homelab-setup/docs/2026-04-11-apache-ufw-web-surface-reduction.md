# Apache Web Server Deployment With UFW Port Restriction

## Overview
This document records the deployment of an Apache HTTP server on Ubuntu Server within a VirtualBox home lab and the application of host-based firewall controls using UFW (Uncomplicated Firewall). The primary objective was to stand up a functional web service while reducing exposure by explicitly permitting only the required inbound ports for HTTP/HTTPS.

## Environment
- **Virtualization:** VirtualBox
- **Guest OS:** Ubuntu Server
- **Service:** Apache HTTP Server (`apache2`)
- **Host Firewall:** UFW

## Implementation Details

### 1) Environment Provisioning & Package Installation
Apache was installed using the Ubuntu package manager to ensure system-managed updates and integrity via repository signatures.

```bash
sudo apt install apache2
