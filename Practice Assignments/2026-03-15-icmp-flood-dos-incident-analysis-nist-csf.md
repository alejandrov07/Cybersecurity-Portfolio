# ICMP Flood Denial of Service Incident Analysis Using NIST CSF

## Overview
This report documents the analysis of a Denial of Service (DoS) incident that disrupted internal network operations at a multimedia services company. The attack exploited an unconfigured firewall to flood the internal network with ICMP traffic, overwhelming network capacity and preventing legitimate internal communication for approximately two hours.

The incident was analyzed using the NIST Cybersecurity Framework (CSF) to assess organizational response, identify security gaps, and define corrective controls to improve resilience against similar attacks.

Completed in March 2026 (exact day not recorded).

## Environment
- Attack Type: ICMP Flood (DoS)
- Affected Scope: Internal corporate network
- Security Framework: NIST Cybersecurity Framework (Identify, Protect, Detect, Respond, Recover)
- Primary Control Gap: Unconfigured firewall

## Incident Summary
A malicious actor launched an ICMP flood attack by sending a large volume of ping requests through the organization’s perimeter firewall. Due to the absence of ICMP filtering and rate-limiting controls, the internal network became saturated, causing all network services to become unavailable.

The disruption lasted approximately two hours and affected all internal users until mitigation actions were applied.

## NIST CSF Incident Analysis

### Identify
The incident management team confirmed a DoS attack after observing a complete loss of internal network connectivity. Investigation determined that an external actor exploited the lack of firewall controls to inject excessive ICMP traffic directly into the internal network, exhausting available bandwidth and processing capacity.

### Protect
Following the incident, multiple protective controls were implemented to reduce exposure to similar attacks:
- Firewall rules limiting the rate of inbound ICMP traffic
- Source IP verification to prevent spoofed ICMP packets
- Deployment of continuous network monitoring tools
- Installation of an IDS/IPS system to automatically filter malicious traffic

### Detect
Detection capabilities were enhanced through:
- An Intrusion Detection System (IDS) to monitor traffic patterns in real time
- Deployment of a Next-Generation Firewall (NGFW) with deep packet inspection and application-layer visibility

These controls significantly improve early detection of anomalous traffic volumes indicative of DoS activity.

### Respond
Immediate response actions included:
- Blocking all inbound ICMP traffic at the firewall
- Taking non-critical services offline to reduce load
- Prioritizing restoration of critical internal services

Post-incident response actions focused on permanent control implementation to prevent recurrence.

### Recover
Recovery efforts prioritized restoring critical services once the attack was contained. Non-critical services were restored only after confirming network stability.

The incident and corrective actions were communicated internally to ensure operational awareness. Newly implemented security controls and response procedures were documented to improve readiness for future incidents.

## Security Insight
ICMP flood attacks demonstrate how even simple protocols can be weaponized when perimeter defenses are misconfigured or absent. While ICMP is essential for network diagnostics, unrestricted ICMP traffic can be leveraged to exhaust network resources and disrupt operations without exploiting application-level vulnerabilities.

This incident underscores the importance of baseline firewall hardening, protocol-level rate limiting, and continuous traffic monitoring. Availability-focused attacks require proactive defensive configurations; reactive blocking alone increases recovery time and business impact.

*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in network traffic analysis and DoS attack identification.*
``
