# 🔍 Investigation 02 — Protocol Analysis

## 📌 Overview

This investigation analyzes the **protocol hierarchy** of the captured network traffic using **Wireshark → Statistics → Protocol Hierarchy**.

The objective is to identify the protocols present, understand their encapsulation structure, examine protocol distribution, and establish a baseline for deeper TCP/UDP and application-layer investigations.

> **Note:** Detailed observations, answers, and analyst assessment are documented in [`findings.md`](./findings.md).

---

## 🎯 Objectives

- Identify protocols present in the PCAP.
- Analyze protocol hierarchy and encapsulation.
- Examine TCP vs UDP distribution.
- Identify application-layer protocols.
- Identify tunneling, authentication, and network-management protocols.
- Determine traffic areas requiring deeper investigation.

---

## 🛠️ Tools & Data

| Component | Details |
|---|---|
| **Tool** | Wireshark |
| **Analysis** | Statistics → Protocol Hierarchy |
| **Capture** | `nb6-startup.pcap` |
| **Total Frames** | 531 |
| **Encapsulation** | Ethernet |

---

## 🌐 Protocol Hierarchy

```text
Frame
└── Ethernet
    ├── PPP-over-Ethernet Session
    │   └── Point-to-Point Protocol
    │       ├── PPP Link Control Protocol
    │       ├── PPP IPv6 Control Protocol
    │       ├── PPP IP Control Protocol
    │       └── PPP Challenge Handshake Authentication Protocol
    │
    ├── PPP-over-Ethernet Discovery
    │
    └── Internet Protocol Version 4
        ├── User Datagram Protocol
        │   ├── Syslog
        │   ├── SIP
        │   ├── NTP
        │   └── L2TP
        │       └── Point-to-Point Protocol
        │           ├── PAP
        │           ├── LCP
        │           ├── IPv6CP
        │           └── IPCP
        │
        ├── DHCP
        ├── DNS
        ├── DHCPv6
        │
        └── Transmission Control Protocol
            └── HTTP
                ├── Unreassembled Fragmented Packet
                └── XML

    ├── Internet Protocol Version 6
    │   └── ICMPv6
    ├── IGMP
    ├── ICMP
    └── ARP

| Protocol        | Packets | % Packets |
| --------------- | ------: | --------: |
| IPv4            |     370 |     69.7% |
| PPPoE Session   |     266 |     50.1% |
| UDP             |     249 |     46.9% |
| TCP             |     116 |     21.8% |
| DNS             |     112 |     21.1% |
| ARP             |      89 |     16.8% |
| L2TP            |      86 |     16.2% |
| HTTP            |      39 |      7.3% |
| NTP             |      34 |      6.4% |
| PPPoE Discovery |      16 |      3.0% |
| DHCP            |      11 |      2.1% |
| IPv6            |      10 |      1.9% |
| ICMPv6          |       6 |      1.1% |
| XML             |       6 |      1.1% |
| SIP             |       4 |      0.8% |
| DHCPv6          |       4 |      0.8% |
| IGMP            |       3 |      0.6% |
| ICMP            |       2 |      0.4% |
| Syslog          |       2 |      0.4% |
| PAP             |       2 |      0.4% |



🔎 Major Observations
Network & Transport
IPv4: 370 packets (69.7%)
IPv6: 10 packets (1.9%)
UDP: 249 packets (46.9%)
TCP: 116 packets (21.8%)
ARP: 89 packets (16.8%)
Application & Services
DNS: 112 packets (21.1%)
HTTP: 39 packets (7.3%)
NTP: 34 packets (6.4%)
DHCP: 11 packets (2.1%)
SIP: 4 packets (0.8%)
Syslog: 2 packets (0.4%)
Tunneling & PPP
PPPoE Session: 266 packets (50.1%)
PPPoE Discovery: 16 packets (3.0%)
L2TP: 86 packets (16.2%)
Nested PPP traffic includes PAP, LCP, IPv6CP, and IPCP.


❓ Investigation Questions

The following questions are answered in findings.md:

What protocols are present in the capture?
Which network-layer protocol is most prominent?
Which transport protocol has more observed packets: TCP or UDP?
Is DNS traffic present?
Is HTTP traffic present?
Is ARP traffic present?
Is IPv6 traffic present?
Is PPPoE traffic present?
Is tunneling traffic present?
Does the protocol hierarchy alone indicate malicious activity?


#Investigation Workflow

Protocol Hierarchy
       ↓
Identify Protocols
       ↓
Analyze Distribution
       ↓
Identify Important Services
       ↓
Select Traffic for Deeper Analysis
       ↓
TCP / UDP Analysis
       ↓
DNS / HTTP Analysis
       ↓
Security Assessment
