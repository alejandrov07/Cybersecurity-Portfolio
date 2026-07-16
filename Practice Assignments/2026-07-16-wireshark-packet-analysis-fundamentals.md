# Wireshark Packet Analysis and Traffic Filtering

## Security Insight
From a defensive posture, relying on raw, unfiltered packet captures (PCAPs) during an active incident response is equivalent to searching for a needle in a haystack. Effective security analysts do not manually read millions of frames; instead, they construct precise logical display filters to isolate anomalous behavior. 

This analysis highlights two critical security architectural weaknesses in the captured environment:
1. **Unencrypted HTTP Traffic (TCP/80):** Raw application data—including the specific client agent (`curl`)—was fully exposed in the packet payload. In a real-world scenario, this lack of transport-layer encryption allows adversaries to perform unauthorized credential harvesting and session hijacking.
2. **Cleartext DNS Queries (UDP/53):** Resolving `opensource.google.com` in cleartext exposes the organization's internal browsing habits and asset landscape to passive eavesdropping, enabling targeted reconnaissance or DNS spoofing attacks.

---

## Technical Execution

The network traffic analysis was performed systematically using Wireshark display filters to isolate, dissect, and inspect layered encapsulation protocols (Ethernet, IPv4, UDP/TCP).

### 1. Host and Protocol Isolation
To isolate traffic involving the target web server, we filter exclusively for its IP address:

```sql
/* Display filter to isolate all traffic originating from or destined to the target server */
ip.addr == 142.250.1.139
```

To narrow down the dataset and verify incoming traffic patterns, specific directional source and destination filters were evaluated:

```sql
/* Isolate traffic sent exclusively by the target host */
ip.src == 142.250.1.139

/* Isolate traffic sent exclusively to the target host */
ip.dst == 142.250.1.139
```

### 2. Physical Layer Correlation
Using the Hardware MAC address filter, we isolated layer-2 frames to correlate physical interfaces with upstream layer-3 protocols:

```sql
/* Track frames linked to the specific network interface card */
eth.addr == 42:01:ac:15:e0:02
```

### 3. DNS Resolution Verification
By isolating User Datagram Protocol (UDP) destination/source ports associated with Domain Name System (DNS) queries, we extracted the resolved mapping for `opensource.google.com` showing response payloads containing `142.250.1.139`:

```sql
/* Filter UDP port 53 traffic to analyze DNS Queries and Answers */
udp.port == 53
```

### 4. TCP Payload Inspection
To detect potentially malicious automated command-line requests or unauthorized scraping tools interacting with our web services, we targeted Transmission Control Protocol (TCP) port 80 and searched the application layer payload for specific user-agent strings:

```sql
/* Filter for standard HTTP web traffic */
tcp.port == 80

/* Inspect raw TCP payload for signatures containing 'curl' */
tcp contains "curl"
```

---

*This project is part of my cybersecurity portfolio*
