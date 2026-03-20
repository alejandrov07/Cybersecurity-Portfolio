# 🛡️ NIST CSF Incident Report: ICMP Flood DoS Attack

**Course:** Google Cybersecurity Certificate  
**Type:** Incident Report Analysis — NIST Cybersecurity Framework  
**Framework:** NIST CSF (Identify, Protect, Detect, Respond, Recover)  
**Attack Type:** DoS — ICMP Flood  

---

## 📋 Scenario

A multimedia company offering web design, graphic design, and social media marketing services experienced a Denial of Service (DoS) attack that disrupted internal network operations for approximately two hours. A malicious actor exploited an unconfigured firewall to flood the company's network with ICMP ping packets, overwhelming the network and preventing normal internal traffic from accessing any network resources. As a cybersecurity analyst, I was tasked with analyzing the incident using the NIST Cybersecurity Framework (CSF) and creating a plan to improve the company's network security.

---

## 📋 Summary

The incident management team responded by blocking incoming ICMP packets, taking non-critical services offline, and prioritizing the restoration of critical services. Following the incident, the cybersecurity team implemented a series of new security controls to strengthen the network and prevent similar attacks in the future, including firewall rule updates, source IP verification, network monitoring software, and an IDS/IPS system.

---

## 📄 Incident Report Analysis

### 🔍 Identify

The incident management team identified that the organization had experienced a DoS attack. Investigation revealed that a malicious actor had sent a flood of ICMP ping packets into the company's internal network through an unconfigured firewall. This vulnerability allowed the attacker to overwhelm the network's capacity, causing all network services to stop responding and preventing legitimate internal traffic from accessing any resources for approximately two hours.

---

### 🔐 Protect

To protect the network against future attacks of this nature, the team implemented the following security controls:

- **New firewall rule** to limit the rate of incoming ICMP packets, preventing a single source from flooding the network
- **Source IP address verification** on the firewall to detect and block spoofed IP addresses on incoming ICMP packets
- **Network monitoring software** to provide continuous visibility into traffic patterns and detect anomalies
- **IDS/IPS system** to automatically filter out suspicious ICMP traffic based on known attack characteristics

---

### 📡 Detect

To improve the organization's ability to detect future incidents, the team will use an **IDS (Intrusion Detection System)** tool to continuously monitor all incoming network traffic for suspicious activity. Additionally, a **Next-Generation Firewall (NGFW)** will be deployed as network monitoring software, providing advanced threat detection capabilities such as deep packet inspection, application-layer filtering, and real-time traffic analysis — significantly improving the organization's ability to identify and respond to threats before they cause disruption.

---

### 🚨 Respond

Upon detecting the attack, the incident management team took the following immediate response actions:

- Blocked all incoming ICMP packets to stop the flood and neutralize the attack
- Took all non-critical network services offline to reduce the attack surface and free up resources
- Restored critical network services to minimize business disruption

Following the incident, the team implemented new long-term security measures to reduce the risk of recurrence:

- Configured new firewall rules to limit and filter ICMP traffic
- Configured the firewall to verify source IP addresses and prevent IP spoofing
- Deployed an IDS/IPS system to automatically detect and filter malicious ICMP traffic

---

### ♻️ Recover

To recover from this DoS attack, the first priority was restoring the critical network services that were taken offline during the incident. Non-critical services remained offline until the attack was fully contained and the network was confirmed stable, ensuring that restoration efforts were not interrupted by residual malicious traffic.

Once critical services were restored and the network returned to normal operation, the incident was communicated internally to all relevant staff and stakeholders. This communication included a summary of what happened, which systems were affected, the duration of the disruption, and the actions taken to resolve it. Transparent internal communication ensures that all teams are aware of the incident and can take any necessary follow-up actions within their areas.

Finally, the new security measures implemented by the network security team were documented and communicated internally, so all relevant personnel understand the controls now in place and the procedures to follow in the event of a future incident.

---

*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in incident analysis using the NIST Cybersecurity Framework (CSF).*
