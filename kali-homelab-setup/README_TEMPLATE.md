# Kali Linux Home Lab - Isolated NAT Network Setup

**Version:** 1.0  
**Status:** ✅ Active & Verified  
**Last Updated:** 2026-04-08

---

## 📋 Project Overview

This repository documents the complete setup, configuration, and deployment of an isolated Kali Linux home laboratory environment for Blue Team cybersecurity training and threat analysis. The lab uses a NAT-isolated network segment to ensure safe, controlled experimentation with network security tools and vulnerability assessment techniques.

**Use Case:** Foundation for DNS exfiltration detection, GRC assessments, CVE analysis, and threat hunting labs.

---

## 🖥️ Technical Specifications

| Component | Version | Details |
|-----------|---------|---------|
| **Host OS** | Windows 10/11 | x64 architecture |
| **Hypervisor** | VirtualBox 7.2.6 r172322 | Oracle VM virtualization |
| **Guest OS** | Kali Linux 2026.1 Rolling | Debian-based penetration testing distribution |
| **Network Mode** | NAT Network | Isolated 10.0.2.0/24 subnet |
| **Kernel** | 6.18.12+kali-amd64 | Linux kernel |
| **RAM Allocated** | 4GB (configurable) | Recommended minimum |
| **Storage** | 40GB+ | Sufficient for tools & labs |

---

## 🏗️ Network Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Windows Host Machine                     │
│                  (Internet Connection: Real)                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │        VirtualBox Hypervisor                          │  │
│  │                                                        │  │
│  │  ┌─────────────────────────────────────────────────┐ │  │
│  │  │   NAT Network: 10.0.2.0/24 (Isolated)          │ │  │
│  │  │                                                  │ │  │
│  │  │  ┌────────────────────────────────────────────┐│ │  │
│  │  │  │  Kali Linux VM                             ││ │  │
│  │  │  │  eth0: 10.0.2.15/24                        ││ │  │
│  │  │  │  MAC: 08:00:27:cb:21:0b                    ││ │  │
│  │  │  │  Gateway: 10.0.2.2                         ││ │  │
│  │  │  │  DNS: Host-provided                        ││ │  │
│  │  │  │                                             ││ │  │
│  │  │  │  Services:                                  ││ │  │
│  │  │  │  • Bash Terminal                           ││ │  │
│  │  │  │  • Penetration Testing Tools               ││ │  │
│  │  │  │  • Network Analysis Tools                  ││ │  │
│  │  │  │  • Threat Detection Labs                   ││ │  │
│  │  │  └────────────────────────────────────────────┘│ │  │
│  │  │                                                  │ │  │
│  │  │  (Future: Add Metasploitable 2, Wazuh, etc)   │ │  │
│  │  └─────────────────────────────────────────────────┘ │  │
│  │                                                        │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
└─────────────────────────────────────────────────────────────┘

ISOLATION MODEL:
• Kali VM ←→ Host via NAT Gateway (10.0.2.2)
• Kali VM ←→ Internet via Host (one-way proxy)
• Kali VM ↔ Physical Network: BLOCKED (isolated)
• Host ↔ Kali VM: ACCESSIBLE (for management)
```

---

## 📧 Contact & Support

**Project Owner:** Alejandro Velazquez
**Specialization:** Blue Team / Cybersecurity Analysis  
**Location:** Santo Domingo, Dominican Republic  

**Goal:** Secure internship in cybersecurity, target: High-Level Blue Team role within 6 months

---

## 📄 License

This documentation and scripts are provided as-is for educational purposes.

---

**Last Verified:** 2026-04-08  
**Status:** ✅ Active & Production-Ready  
**Snapshot:** clean-kali-base (Available for rollback)
