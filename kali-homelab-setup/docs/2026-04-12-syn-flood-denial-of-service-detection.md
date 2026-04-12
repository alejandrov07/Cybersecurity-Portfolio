<!--
Location: kali-homelab-setup/docs/
Filename: 2026-04-12-syn-flood-denial-of-service-detection.md
-->

# Detection and Response to TCP SYN Flood Denial-of-Service Attack

## Executive Summary
This report documents the detection, analysis, and initial response to a denial-of-service condition affecting a public-facing web server used by a travel agency to advertise sales and promotions. The incident resulted in service unavailability for internal employees and external users due to an abnormal volume of TCP SYN requests originating from an unfamiliar source.

## Incident Context
- **Affected Asset:** Public web server hosting sales and promotional content
- **Users Impacted:** Company employees and website visitors
- **Initial Symptom:** Website connection timeouts
- **Alert Source:** Automated monitoring system

## Detection & Analysis

### Network Observation
Packet capture analysis revealed a sustained flood of **TCP SYN packets** targeting the web server from a single external IP address. The server was unable to complete the TCP three-way handshake at scale, leading to resource exhaustion and degraded responsiveness.

### Attack Characterization
The traffic pattern is consistent with a **TCP SYN Flood Denial-of-Service (DoS) attack**, where the attacker overwhelms the server’s connection table (SYN backlog) by initiating large volumes of half-open TCP connections.

Key indicators:
- High volume of SYN packets
- Incomplete TCP handshakes
- Legitimate traffic experiencing timeouts
- Service availability degradation

## Immediate Response Actions
1. **Service Containment**
   - The affected server was temporarily taken offline to allow recovery and stabilization.

2. **Reactive Mitigation**
   - Firewall rules were applied to block the offending IP address responsible for the abnormal traffic volume.

## Limitations of Initial Mitigation
While IP-based blocking provided short-term relief, this control is inherently fragile. A malicious actor can:
- **Spoof source IP addresses**
- Rotate attack sources
- Distribute the attack across multiple hosts

As a result, simple IP blocking does not scale against volumetric or distributed denial-of-service techniques.

## Security Insight (Attack Surface & Availability Risk)
This incident demonstrates how denial-of-service attacks directly threaten **availability**, one of the core pillars of the CIA triad. The most critical impact was operational: employees could not access sales content, disrupting business workflows and customer engagement.

Firewalls operating solely on static rules are insufficient against SYN flood attacks. Without protocol-aware protections, the server remains vulnerable even if only a single service (HTTP/HTTPS) is exposed. Effective defense requires controls that understand connection state and traffic behavior, not just source addresses.

## Recommendations (Preventive Controls)

1. **Stateful Firewall & Rate Limiting**
   - Implement SYN rate limiting or connection thresholds to prevent backlog exhaustion.

2. **SYN Cookies / TCP Hardening**
   - Enable kernel-level protections to mitigate half-open connection abuse.

3. **Network-Level DDoS Protections**
   - Consider upstream mitigation (ISP or cloud-based DDoS protection) for volumetric attacks.

4. **Monitoring & Alerting Enhancements**
   - Alert on abnormal SYN rates, not just service downtime.

5. **Defense-in-Depth**
   - Combine firewall rules, host hardening, and traffic analysis rather than relying on IP blocks alone.

## Outcome
The incident was contained, the server recovered, and visibility was gained into a realistic availability-focused attack vector. This event reinforced the importance of layered defenses and proactive network traffic monitoring in protecting business-critical services.
