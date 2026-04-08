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

## 📦 Prerequisites

- VirtualBox 7.0+ installed on Windows host
- Kali Linux ISO (already deployed as VM)
- Minimum 4GB RAM free on host
- 40GB+ free disk space
- Administrator access on host machine

---

## 🚀 Setup Process

### Phase 1: Network Configuration

#### 1.1 Verify VirtualBox Installation

```powershell
# Windows PowerShell (as Administrator)
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" --version
# Output: 7.2.6 r172322
```

#### 1.2 Create/Configure NAT Network in VirtualBox

**On Host (VirtualBox Manager):**

1. File → Preferences → Network → NAT Networks
2. Click **+** (Add NAT Network)
3. Configure:
   - **Network Name:** `NatNetwork`
   - **Network CIDR:** `10.0.2.0/24`
   - **Enable DHCP:** ✓ Checked
4. Click OK

#### 1.3 Assign Kali VM to NAT Network

**In VirtualBox Manager:**

1. Right-click Kali VM → Settings
2. Network → Adapter 1
3. **Attached to:** NAT Network
4. **Name:** NatNetwork
5. Click OK

#### 1.4 Verify Network Inside Kali VM

**Start Kali VM and open Terminal:**

```bash
ip addr
```

**Expected Output:**
```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
```

**Key Verification Points:**
- ✅ Interface eth0 state: UP
- ✅ IP address: 10.0.2.x/24 (within NAT subnet)
- ✅ Broadcast: 10.0.2.255

---

### Phase 2: Fix APT Repository Configuration

#### 2.1 Edit Sources List

```bash
sudo nano /etc/apt/sources.list
```

#### 2.2 Clear and Add Kali Repositories

**In nano editor:**

1. Select all: `Ctrl + A`
2. Delete: `Ctrl + K` (repeat if needed)
3. Paste these lines:

```
deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
deb-src http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

#### 2.3 Save and Exit

- `Ctrl + X`
- `Y` (yes, save)
- `Enter` (confirm filename)

#### 2.4 Update APT Cache

```bash
sudo apt update
```

**Expected Output:** "Reading package lists... Done" (without errors)

---

### Phase 3: Install Guest Additions

#### 3.1 Install Dependencies

```bash
sudo apt install -y build-essential linux-headers-$(uname -r) dkms
```

#### 3.2 Mount and Install Guest Additions

**From VirtualBox Manager:**
1. Kali VM window → Devices → Insert Guest Additions CD Image

**In Kali Terminal:**

```bash
sudo mount /media/cdrom /mnt/cdrom 2>/dev/null || sudo mount /dev/sr0 /mnt/cdrom
cd /mnt/cdrom
sudo bash VBoxLinuxAdditions.run
```

**Expected Output:**
```
Installing the Window System drivers
...
Guest Additions installation completed successfully.
```

#### 3.3 Reboot and Verify

```bash
sudo reboot
```

**After reboot, verify:**

```bash
lsmod | grep vbox
# Expected: vboxguest module loaded

modinfo vboxguest | head -5
# Expected: Module information displayed
```

---

### Phase 4: Create Clean Snapshot

#### 4.1 Power Off VM

```bash
sudo poweroff
```

#### 4.2 Create Snapshot in VirtualBox

**In VirtualBox Manager:**

1. Select Kali VM
2. Click **Snapshots** button (top-right)
3. Click **camera icon** (Take Snapshot)
4. Fill dialog:
   - **Name:** `clean-kali-base`
   - **Description:** `Fresh Kali 2026.1 with NAT, Guest Additions, APT sources configured`
5. Click OK

**Verification:** Snapshot appears in Snapshots panel

---

## ✅ Validation & Verification

### Checklist: All Systems Go

```bash
# 1. Network Isolation
ip addr
# ✅ Shows eth0: 10.0.2.15/24

# 2. Internet Connectivity (via NAT)
ping -c 4 8.8.8.8
# ✅ 4 packets received, 0% loss

# 3. APT Working
apt search curl | head -5
# ✅ Returns package results

# 4. Guest Additions
lsmod | grep vbox
# ✅ vboxguest module listed

# 5. System Info
uname -a
# ✅ Shows kernel 6.18.12+kali-amd64

# 6. Kali Version
cat /etc/os-release | grep VERSION
# ✅ Shows VERSION="2026.1"
```

### Comprehensive Verification Script

See: `scripts/verify-system.sh`

```bash
chmod +x scripts/verify-system.sh
./scripts/verify-system.sh
```

---

## 📊 Test Results

| Test | Command | Status | Output |
|------|---------|--------|--------|
| NAT Network | `ip addr` | ✅ PASS | eth0: 10.0.2.15/24 |
| Internet Access | `ping -c 4 8.8.8.8` | ✅ PASS | 4/4 packets, 39.5ms avg |
| APT Sources | `sudo apt update` | ✅ PASS | No errors |
| Guest Additions | `lsmod \| grep vbox` | ✅ PASS | vboxguest loaded |
| Kernel | `uname -a` | ✅ PASS | 6.18.12+kali-amd64 |
| Kali Version | `cat /etc/os-release` | ✅ PASS | 2026.1 (Rolling) |

---

## 📁 Repository Structure

```
kali-homelab-setup/
├── README.md                          # This file
├── docs/
│   ├── SETUP_GUIDE.md                # Detailed setup instructions
│   ├── NETWORK_CONFIGURATION.md      # Network architecture details
│   ├── TROUBLESHOOTING.md            # Common issues & solutions
│   └── ARCHITECTURE.md               # System design documentation
├── scripts/
│   ├── verify-network.sh             # Network verification script
│   ├── verify-guest-additions.sh     # Guest Additions check
│   └── system-info.sh                # Complete system information
├── screenshots/
│   ├── 01-virtualbox-version.png
│   ├── 02-kali-version-output.png
│   ├── 03-network-nat-adapter.png
│   ├── 04-ip-addr-output.png
│   ├── 05-ping-test-success.png
│   ├── 06-guest-additions-installed.png
│   ├── 07-snapshot-created.png
│   └── 08-final-system-info.png
├── outputs/
│   ├── network-config.txt            # Output of ip addr
│   ├── system-info.txt               # Full system information
│   ├── connectivity-test.txt         # Ping test results
│   └── guest-additions-verification.txt
└── logs/
    └── setup-log-2026-04-08.txt      # Complete setup log
```

---

## 🔍 Key Configuration Files

### /etc/apt/sources.list
Location of Kali package repositories. Configured with:
```
deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

### /etc/network/interfaces
Automatically configured by DHCP in NAT Network mode.

### VirtualBox Network Settings
- Network Mode: NAT Network
- Subnet: 10.0.2.0/24
- Gateway: 10.0.2.2
- DHCP: Enabled

---

## 🛠️ Troubleshooting

### Issue: VM not connecting to network
**Solution:** See `docs/TROUBLESHOOTING.md`

### Issue: APT packages not found
**Solution:** Verify `/etc/apt/sources.list` contains official Kali repositories

### Issue: Guest Additions installation fails
**Solution:** Ensure linux-headers match kernel version with `uname -r`

For more: See `docs/TROUBLESHOOTING.md`

---

## 📈 Next Phases (Roadmap)

- [ ] Phase 5: Install Metasploitable 2 (vulnerable target VM)
- [ ] Phase 6: Deploy Wazuh (SIEM)
- [ ] Phase 7: Deploy Suricata (IDS)
- [ ] Phase 8: DNS Exfiltration Detection Lab
- [ ] Phase 9: GRC Assessment Lab (NIST CSF + PCI-DSS)
- [ ] Phase 10: CVE Analysis Lab
- [ ] Phase 11: Threat Hunting Lab

---

## 💾 How to Use This Repository

### For Beginners
1. Read the main README (you are here)
2. Follow `docs/SETUP_GUIDE.md` step-by-step
3. Reference `screenshots/` for visual verification

### For Experienced Users
1. Review `NETWORK_CONFIGURATION.md` for architecture
2. Run `scripts/verify-system.sh` to validate
3. Customize as needed for your environment

### For Recruiters / Learning Proof
- **Portfolio Evidence:** Complete setup documentation with verification outputs
- **Technical Skills Demonstrated:** 
  - Network isolation & NAT configuration
  - Linux system administration
  - Hypervisor management
  - Package management & APT troubleshooting
  - System verification & validation
- **Blue Team Foundation:** Secure, isolated lab for threat detection training

---

## 📝 Documentation Conventions

All documentation follows:
- **Markdown formatting** for readability
- **Code blocks** with syntax highlighting
- **Screenshot references** in `screenshots/` folder
- **Command examples** with expected output
- **Numbered steps** for sequential processes
- **Verification checkpoints** after critical steps

---

## 🔒 Security Notes

- **Isolation:** Kali VM is isolated from physical network via NAT
- **Host Protection:** Vulnerable VMs (future phases) cannot directly access host network
- **Internet Access:** Available through NAT gateway for updates and research
- **Snapshots:** Allow safe experimentation with quick rollback capability

---

## 📚 References

- [Kali Linux Official Documentation](https://docs.kali.org/)
- [VirtualBox NAT Network Guide](https://www.virtualbox.org/manual/)
- [Blue Team Training Resources](https://www.letsdefend.io/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)

---

## 📧 Contact & Support

**Project Owner:** Alejandro  
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
