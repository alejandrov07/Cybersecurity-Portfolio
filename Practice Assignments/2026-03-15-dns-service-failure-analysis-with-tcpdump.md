# DNS Service Failure Analysis Using ICMP and UDP Traffic

## Overview
This report documents the investigation of a service outage affecting a public-facing website caused by DNS resolution failure. Multiple users were unable to access the website and received destination port unreachable errors when attempting to load the page.

Network traffic was captured and analyzed to identify the affected protocol, determine the root cause, and document the impact from an incident response perspective.

Completed in March 2026 (exact day not recorded).

## Environment
- Network Analysis Tool: tcpdump
- Protocols Analyzed: UDP, DNS, ICMP
- Affected Domain: www.yummyrecipesforme.com
- DNS Server IP: 203.0.113.2
- Client IP: 192.51.100.15

## Incident Summary
The website outage was caused by a failure in DNS resolution. UDP-based DNS queries sent to the DNS server on port 53 consistently resulted in ICMP error messages indicating that the destination port was unreachable. Without successful DNS resolution, client systems were unable to translate the domain name into an IP address, making the website inaccessible.

## Network Traffic Analysis

### DNS Query Behavior
The client system attempted to resolve the domain name by sending UDP packets to the DNS server on port 53 requesting an A record. These queries were transmitted successfully but never processed by the DNS service.

### ICMP Error Responses
Each DNS query attempt resulted in an ICMP error message stating: udp port 53 unreachable

This response confirms that the DNS server received the packets but had no active service listening on the expected port.

### Observed Traffic Pattern
- Three consecutive DNS query attempts were made
- Each attempt generated the same ICMP error
- The issue persisted across multiple minutes, indicating a sustained service failure rather than a transient network issue

## Key Findings

| Indicator | Observation |
|--------|------------|
| Affected Service | DNS |
| Affected Port | UDP 53 |
| DNS Server | 203.0.113.2 |
| Client System | 192.51.100.15 |
| Error Message | ICMP udp port 53 unreachable |
| Failed Attempts | 3 consecutive queries |
| Impact | Complete website inaccessibility |

## Incident Detection
The incident was first reported by multiple customers who were unable to access the website. A cybersecurity analyst independently reproduced the issue and escalated it to the security engineering team.

tcpdump was used to capture live traffic while attempting to access the website, allowing precise identification of protocol behavior, error messages, timestamps, and affected ports.

## Root Cause Assessment
The most likely causes of the DNS failure are:
- The DNS service on the server was stopped or crashed
- A misconfiguration or firewall rule blocked inbound UDP traffic on port 53

In either scenario, the absence of a listening DNS service prevented domain name resolution and caused a full service outage.

## Recommended Remediation Steps
- Verify that the DNS service is running on the affected server
- Review firewall configurations for recent changes affecting UDP port 53
- Inspect server logs for crashes, misconfigurations, or signs of malicious activity
- Restore DNS functionality and monitor traffic for anomalies
- Implement service health monitoring to detect DNS failures earlier

## Security Insight
DNS is a foundational dependency for nearly all web services. When DNS resolution fails, applications may appear offline even if the web server itself is fully operational. ICMP error messages such as “udp port unreachable” provide immediate evidence of service-level failure and should be treated as high-priority indicators during outage investigations.

From a defensive perspective, DNS services must be continuously monitored and protected. Whether caused by misconfiguration, service crashes, or targeted attacks, DNS failures have an outsized impact on availability and user trust, making rapid detection and response essential.

*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in network traffic analysis and DoS attack identification.*
