# SYN Flood Denial of Service Attack Analysis Using Wireshark’s sales webpage. Network traffic was captured and analyzed to determine the root cause, assess impact, and apply containment measures.

The analysis focused on TCP traffic patterns to identify abnormal behavior consistent with a SYN flood attack targeting the HTTPS service.

Completed in March 2026 (exact day not recorded).

## Environment
- Network Monitoring Tool: Wireshark
- Protocols Analyzed: TCP, HTTP
- Target Service: HTTPS (TCP port 443)
- Target Server IP: 192.0.2.1

## Incident Summary
The web server became unavailable due to a high volume of incomplete TCP connection attempts originating from a single external IP address. These half-open connections exhausted the server’s backlog queue, preventing legitimate users from establishing new sessions and resulting in HTTP 504 Gateway Timeout errors.

## Attack Identification
Packet analysis revealed a large number of TCP SYN packets sent from a single source (203.0.113.0) to the web server without completing the TCP three-way handshake. Unlike legitimate clients, the attacker never transmitted the final ACK packet, leaving each connection in a half-open state.

This behavior is consistent with a **SYN Flood Denial of Service (DoS) attack**, which exploits the resource allocation behavior of the TCP handshake to overwhelm a target system.

## Technical Analysis

### Normal TCP Connection Behavior
Under standard conditions, TCP connections are established using a three-step handshake:
1. Client sends SYN to initiate the connection
2. Server responds with SYN/ACK and reserves resources
3. Client replies with ACK, completing the connection

Early packets in the capture show legitimate users successfully completing this process and receiving HTTP 200 OK responses for `/sales.html`.

### SYN Flood Attack Mechanics
During the attack:
- The server responded to each malicious SYN with SYN/ACK
- No ACK was ever received from the attacker
- Each half-open connection consumed server memory and queue space
- The fixed-size backlog queue filled completely
- Legitimate users were rejected with RST/ACK packets or experienced timeouts

### Attack Progression
- **Normal Activity (Packets #47–54):** Legitimate TCP handshakes completed successfully
- **Attack Initiation (Packets #52–83):** SYN packets from 203.0.113.0 increased rapidly
- **Server Degradation (Packet #73):** RST/ACK responses sent to legitimate users
- **Service Failure (Packet #77):** HTTP 504 Gateway Timeout observed
- **Full Saturation (Packet #84 onward):** Traffic dominated by attacker SYN packets only

## Key Findings

| Indicator | Observation |
|--------|------------|
| Attacker IP | 203.0.113.0 |
| Target Server | 192.0.2.1 |
| Target Port | 443 (HTTPS) |
| Attack Type | SYN Flood DoS |
| Attack Start | Packet #52 (3.390692s) |
| Service Impact | HTTP 504 errors, connection resets |
| Traffic Pattern | 100+ SYN packets from a single IP |

## Response Actions
- Took the web server offline temporarily to clear the connection backlog
- Applied a firewall rule blocking traffic from the attacker IP address
- Escalated the incident to security engineering and management teams

## Security Insight
SYN flood attacks exploit a fundamental limitation of the TCP protocol: the need to allocate resources before a connection is fully established. When defenses such as SYN cookies or rate limiting are not in place, even a single source IP can exhaust server capacity with minimal effort.

Blocking an attacker IP provides only short-term relief, as source addresses can be easily spoofed or distributed across multiple hosts. Effective mitigation requires protocol-level protections, adaptive rate controls, and continuous traffic monitoring to detect abnormal handshake patterns before service degradation occurs.

## Recommended Mitigations
- Enable SYN cookies to prevent backlog exhaustion
- Implement firewall rate limiting for inbound SYN packets
- Deploy a Web Application Firewall (WAF) or managed DDoS protection
- Monitor for distributed or spoofed attack patterns
- Review logs for potential secondary impacts during the outage window

*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in network traffic analysis and DoS attack identification.*
``

## Overview
