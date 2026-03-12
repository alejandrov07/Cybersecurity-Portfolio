#🚨 Cybersecurity Incident Report: DNS & ICMP Traffic Analysis

**Course:** Google Cybersecurity Certificate  
**Type:** Network Traffic Analysis — DNS Failure  
**Tool Used:** tcpdump  
**Protocols Analyzed:** UDP, DNS, ICMP

## Scenario
A client's website — www.yummyrecipesforme.com — became inaccessible to multiple users. Customers reported seeing the error "destination port unreachable" when attempting to load the page. As a cybersecurity analyst, I was tasked with identifying the affected network protocol and documenting the incident.

## Tools & Methods
- tcpdump — Network protocol analyzer used to capture and inspect live traffic
- UDP/DNS query — Sent to DNS server to resolve the domain name to an IP address
- ICMP error analysis — Analyzed the return error messages from the DNS server

## Part 1: DNS & ICMP Traffic Log Summary
The UDP protocol reveals that:
The local machine (192.51.100.15) attempted to resolve the domain yummyrecipesforme.com by sending a UDP packet to the DNS server (203.0.113.2) on port 53. The DNS query requested an A record — a mapping between the domain name and its corresponding IP address.
The network analysis shows that the ICMP error message returned was:
udp port 53 unreachable
This indicates that the DNS server received the UDP packet but had no active service listening on port 53 to process the request. The ICMP message was returned three consecutive times, confirming the issue was persistent and not a temporary glitch.
Port 53 is used for:
DNS (Domain Name System) — the service responsible for translating human-readable domain names (e.g., www.yummyrecipesforme.com) into machine-readable IP addresses. Without DNS resolution, browsers cannot locate the web server to load the website.
The most likely issue is:
Either the DNS service on the server has crashed/stopped running, or a firewall rule is blocking incoming traffic on UDP port 53, preventing DNS queries from being processed.

## Part 2: Incident Analysis
FieldDetailsTime of Incident13:24:32 PM (first logged event)Affected HostDNS Server — 203.0.113.2Source IP192.51.100.15 (analyst's machine)Port AffectedUDP Port 53 (DNS)Error ReturnedICMP udp port 53 unreachableNumber of Failed Attempts3
How the IT team became aware of the incident:
Multiple customers reported being unable to access www.yummyrecipesforme.com and receiving the error "destination port unreachable." A cybersecurity analyst independently confirmed the issue by attempting to visit the site and receiving the same error. The incident was then escalated to the security engineering team.
Actions taken by the IT department to investigate:
The analyst launched tcpdump, a network protocol analyzer, and attempted to load the website again while capturing traffic. The tool logged all outgoing UDP packets and incoming ICMP responses, allowing the team to identify the source IP, destination IP, timestamps, port numbers, and the specific error message at each step of the communication.
Key findings of the investigation:

The DNS server at 203.0.113.2 was not responding to queries on UDP port 53
Three separate DNS query attempts were sent at 13:24, 13:26, and 13:28 — all returned the same ICMP error
The error udp port 53 unreachable confirms no service was actively listening on port 53 at the time
Without a successful DNS response, the browser could not resolve the domain to an IP address, making the website completely inaccessible

Most likely cause of the incident:
The DNS service on the server (203.0.113.2) had stopped running — either due to a crash, misconfiguration, or a potential malicious attack targeting the DNS infrastructure. A secondary possibility is that a firewall rule was modified to block UDP port 53, preventing DNS traffic from reaching the service.
Recommended next steps:

Verify whether the DNS service is running on 203.0.113.2
Review firewall rules for any recent changes blocking port 53
Check server logs for signs of a DoS/DDoS attack or unauthorized configuration changes
Restore DNS service and monitor traffic for anomalies


## 🗂️ tcpdump Log
13:24:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com. (24)
13:24:36.098564 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 254

13:26:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com. (24)
13:27:15.934126 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 320

13:28:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com. (24)
13:28:50.022967 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 150
