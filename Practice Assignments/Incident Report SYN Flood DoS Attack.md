# 🚨 Cybersecurity Incident Report: SYN Flood DoS Attack

**Course:** Google Cybersecurity Certificate  
**Type:** Network Traffic Analysis — DoS Attack  
**Tool Used:** Wireshark (Packet Sniffer)  
**Protocols Analyzed:** TCP, HTTP  

---

## 📋 Scenario

Employees and customers of a travel agency reported being unable to access the company's sales webpage. The web server began returning connection timeout errors. As a cybersecurity analyst, I captured network traffic using a packet sniffer, analyzed the logs, identified the attack type, took the server offline to allow recovery, and configured the firewall to block the malicious IP address.

---

## 🧰 Tools & Methods

- **Wireshark** — Packet sniffer used to capture and inspect TCP traffic in real time
- **TCP handshake analysis** — Used to distinguish legitimate connections from malicious SYN flood activity
- **Firewall configuration** — Applied IP block rule against the attacker's source address

---

## 📄 Part 1: Attack Identification

**One potential explanation for the website's connection timeout error is:**  
The web server (`192.0.2.1`) became overwhelmed by an abnormally large volume of TCP SYN requests originating from a single IP address (`203.0.113.0`). As the server's connection queue filled up with half-open connections waiting for ACK responses that never arrived, it lost its ability to process requests from legitimate users — causing the server to return a **504 Gateway Timeout** error.

**The logs show that:**  
Starting at packet #52, IP address `203.0.113.0` began sending a high volume of TCP SYN packets to the web server on port 443. Unlike legitimate users — who completed the full three-way handshake (SYN → SYN/ACK → ACK) — this IP never sent the final ACK, leaving connections in a half-open state. The frequency of these SYN packets escalated rapidly, eventually dominating the entire log from packet #83 onward.

**This event is classified as:**  
A **SYN Flood Attack** — a type of **Denial of Service (DoS)** attack that exploits the TCP three-way handshake to exhaust server resources and deny service to legitimate users.

---

## 📄 Part 2: Attack Analysis & Impact on the Web Server

### The TCP Three-Way Handshake (Normal Behavior)

Under normal conditions, establishing a TCP connection requires three steps:

| Step | Direction | Description |
|------|-----------|-------------|
| **1. SYN** | Client → Server | The client sends a SYN packet to initiate a connection |
| **2. SYN/ACK** | Server → Client | The server acknowledges and reserves resources for the connection |
| **3. ACK** | Client → Server | The client confirms — the connection is fully established |

This process can be observed in the early log entries with legitimate users (e.g., packets #47–51 for `198.51.100.23`, packets #55–62 for `198.51.100.14`).

---

### What Happens During a SYN Flood Attack

When a malicious actor sends a large number of SYN packets without ever completing the handshake:

1. The server receives each SYN and responds with SYN/ACK, allocating memory and resources for each half-open connection
2. The attacker never sends the final ACK, so those connections remain open and consume server resources indefinitely
3. The server's **backlog queue** — which has a fixed capacity — fills up entirely with these incomplete connections
4. Once the queue is full, the server can no longer accept new connections from legitimate users
5. The server begins rejecting connections with **RST/ACK** packets and eventually stops responding altogether

---

### Log Analysis: Attack Progression

**Phase 1 — Normal activity (packets #47–54):**  
Legitimate users successfully complete the TCP three-way handshake and receive HTTP 200 OK responses for `/sales.html`.

**Phase 2 — Attack begins (packets #52–83):**  
`203.0.113.0` starts sending SYN packets from port `54770`. Initially the server handles both legitimate and malicious traffic, but the volume of SYN requests from the attacker grows rapidly. At packet #73, the server begins sending `RST/ACK` responses to legitimate users — indicating it is already struggling to maintain normal connections.

**Phase 3 — Server degradation (packets #77–83):**  
At packet #77, the server returns a **HTTP 504 Gateway Timeout** to a legitimate user (`198.51.100.5`). By packet #83, the server starts sending `RST/ACK` responses directly to the attacker (`203.0.113.0`), attempting to reset the flood of half-open connections — but it is too late.

**Phase 4 — Full saturation (packets #84 onward):**  
From packet #84 to the end of the log, the traffic is almost exclusively SYN packets from `203.0.113.0`. Legitimate users receive `RST/ACK` rejections (packets #85, #92, #121). The server is completely overwhelmed and unable to serve any legitimate requests.

---

### Key Findings

| Field | Details |
|---|---|
| **Attacker IP** | `203.0.113.0` |
| **Target Server** | `192.0.2.1` (port 443 — HTTPS) |
| **Attack Type** | SYN Flood — Denial of Service (DoS) |
| **Attack Start** | Packet #52 — timestamp 3.390692s |
| **Server degradation begins** | Packet #73 — RST/ACK sent to legitimate user |
| **504 Gateway Timeout** | Packet #77 |
| **Full saturation** | Packet #84 onward — only attacker traffic visible |
| **Total malicious SYN packets** | 100+ from a single IP address |

---

## 🛡️ Response Actions Taken

1. **Took the web server offline temporarily** to allow it to clear its backlog queue and recover to a normal operating state
2. **Configured the firewall** to block all incoming traffic from `203.0.113.0`
3. **Escalated the incident** to the security engineering team and management

---

## ⚠️ Limitations & Next Steps

The IP block applied is a **temporary measure**. A determined attacker can bypass it by **spoofing different source IP addresses**, launching the same attack from new IPs.

**Recommended next steps:**

1. Implement **SYN cookies** on the web server to handle half-open connections without consuming backlog memory
2. Deploy a **rate-limiting rule** on the firewall to cap the number of SYN packets accepted per second from any single IP
3. Consider a **Web Application Firewall (WAF)** or **DDoS protection service** (e.g., Cloudflare) for long-term mitigation
4. Monitor traffic continuously for signs of IP spoofing or distributed attack patterns (DDoS)
5. Review server logs to determine whether any data was accessed or exfiltrated during the attack window

---

## 🗂️ Wireshark Log Summary (Key Packets)

```
# Normal traffic — Three-way handshake (legitimate user)
47  3.144521  198.51.100.23  192.0.2.1  TCP   [SYN]
48  3.195755  192.0.2.1      198.51.100.23  TCP   [SYN, ACK]
49  3.246989  198.51.100.23  192.0.2.1  TCP   [ACK]
50  3.298223  198.51.100.23  192.0.2.1  HTTP  GET /sales.html
51  3.349457  192.0.2.1      198.51.100.23  HTTP  200 OK

# Attack begins — SYN flood from 203.0.113.0
52  3.390692  203.0.113.0    192.0.2.1  TCP   [SYN] ← attacker starts
57  3.664863  203.0.113.0    192.0.2.1  TCP   [SYN] ← frequency increases
66  4.256159  203.0.113.0    192.0.2.1  TCP   [SYN]

# Server begins to fail
73  6.230548  192.0.2.1      198.51.100.16  TCP  [RST, ACK] ← legitimate user rejected
77  7.330577  192.0.2.1      198.51.100.5   TCP  HTTP 504 Gateway Timeout
83  7.482754  192.0.2.1      203.0.113.0    TCP  [RST, ACK] ← server attempts reset

# Full saturation — only attacker traffic from packet 84 onward
84–214+  203.0.113.0  192.0.2.1  TCP  [SYN] (continuous flood)
```


*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in network traffic analysis and DoS attack identification.*
