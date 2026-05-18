# SOC Lab - Day 1: Network Baseline & Traffic Capture

**Objective:** Establish a baseline of normal network traffic from a freshly-installed Ubuntu Server in an isolated lab environment. This baseline will be used in subsequent labs to identify anomalies via statistical deviation.

**MITRE ATT&CK:** T1046 - Network Service Discovery
**Date Completed:** May 18, 2026
**Lab Status:** Complete

---

## Topology

Workstation (32GB RAM, 12 cores)
└── VirtualBox 7.x
    └── NAT Network: SOCLab (10.0.2.0/24)
        ├── SOCLab-Ubuntu (10.0.2.15)
        ├── SOCLab-Kali (10.0.2.16) [Day 2+]
        └── SOCLab-Windows (10.0.2.17) [Day 3+]

---

## Methodology

### Phase 1: Network Setup
1. Created VirtualBox NAT network SOCLab (10.0.2.0/24)
2. Deployed Ubuntu Server 22.04 VM (4GB RAM, 30GB disk)
3. Installed tools: tcpdump, tshark, dnsutils, curl
4. Configured SSH port forwarding (Host 22 -> Guest 22)

### Phase 2: Baseline Traffic Generation
1. Started capture: sudo tcpdump -i enp0s3 -w baseline.pcap
2. Generated normal traffic:
   - DNS queries (nslookup google.com, github.com)
   - HTTPS requests (curl to google.com)
   - SSH connection (localhost)
   - ICMP ping (gateway 10.0.2.1 and 8.8.8.8)
3. Captured for ~10 minutes total

### Phase 3: Automated Analysis
1. Transferred baseline.pcap to Windows via SCP
2. Analyzed with Python + Scapy
3. Generated JSON report with metrics
4. Produced anomaly thresholds

---

## Evidence

### baseline.pcap Properties

File Size: 2.1 MB
Packet Count: 12,765
Capture Interface: enp0s3 (NAT)
PCAP Version: 2.4 (microsecond timestamp)
Duration: ~10 minutes

### Protocol Distribution

TCP: 11,622 packets (91.0%)
UDP: 151 packets (1.2%)
ICMP: 10 packets (0.1%)

### Top IP Addresses

10.0.2.15: 11,783 packets (Ubuntu VM, our target)
10.0.2.2: 11,622 packets (VirtualBox NAT gateway)
192.168.1.1: 94 packets (Local gateway background traffic)
91.189.91.112: 15 packets (Ubuntu mirrors)
8.8.8.8: 10 packets (Google DNS ping responses)

### Top Destination Ports

Port 22 (SSH): 7,568 packets
Port 53145 (ephemeral/tunneled): 4,054 packets
Port 53 (DNS): 47 packets
Port 123 (NTP): 31 packets

### DNS Queries Captured

github.com: 43 queries
google.com: 2 queries
www.google.com: 2 queries

---

## Anomaly Thresholds (for Day 2+)

DNS Exfiltration:
- Normal: 3 unique domains, 47 queries in 10 min
- Anomaly threshold: >20 unique domains in 10 min OR >15 queries/min with FQDN length >63 chars

Port Scan Detection:
- Normal: 77 unique destination ports over 10 min
- Anomaly threshold: >50 unique ports in 1 minute

Protocol Anomaly:
- Normal: TCP 91%, UDP 1%, ICMP <1%
- Anomaly threshold: UDP >50% in 5-min window OR ICMP >20%

Packet Size Anomaly:
- Normal: Average 153 bytes
- Anomaly threshold: Average <100 bytes OR >1000 bytes

---

## Mitigations

1. This baseline will serve as reference for Days 2-14
2. Use these metrics to tune IDS/IPS sensors and reduce false positives
3. NIST SP 800-137 states baselining is foundation of monitoring
4. Establish detection rules based on deviations from this baseline

---

## Commands Used

### Ubuntu VM
sudo tcpdump -i enp0s3 -w baseline.pcap &
for i in {1..20}; do nslookup google.com; sleep 0.5; done
for i in {1..10}; do curl -s https://www.google.com > /dev/null; sleep 1; done
ssh -o StrictHostKeyChecking=no alejandro69@localhost "echo 'SSH test'" 2>/dev/null || true
ping -c 10 10.0.2.1
ping -c 5 8.8.8.8 || true
sleep 300
pkill -f "tcpdump -i enp0s3"

### Windows Workstation
scp alejandro69@localhost:baseline.pcap .
python baseline_analyzer.py --analyze baseline.pcap --report baseline_report.json --verbose

---

## Lab Checklist

[x] VirtualBox NAT network created
[x] Ubuntu Server 22.04 VM deployed
[x] Network tools installed
[x] baseline.pcap captured (12,765 packets, 2.1 MB)
[x] baseline_report.json generated
[x] README.md written with evidence
[x] Anomaly thresholds established

---

Lab Status: Complete
Analyst: Alejandro
Date: May 18, 2026
Next: Day 2 - SSH Brute Force Detection
