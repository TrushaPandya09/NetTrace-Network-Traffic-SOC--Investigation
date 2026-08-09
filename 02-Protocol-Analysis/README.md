# 02 — Protocol Analysis

## 1. Overview

This phase analyzes the network protocols observed in the captured PCAP file using Wireshark. The objective is to understand the protocol distribution, communication layers, and major application protocols involved in the captured traffic.

The analysis helps establish a baseline of normal and potentially suspicious network activity before moving into deeper traffic investigations.

---

## 2. Objectives

- Identify the protocols present in the PCAP.
- Determine the protocol hierarchy and traffic distribution.
- Identify the dominant network and transport-layer protocols.
- Identify application-layer protocols such as HTTP, DNS, TLS, etc.
- Understand how the protocols are layered during communication.
- Identify any unusual or unexpected protocols that may require further investigation.

---

## 3. Tool Used

| Tool | Purpose |
|---|---|
| Wireshark | Packet capture analysis and protocol inspection |


---

## 4. Protocol Hierarchy Analysis

Wireshark's **Protocol Hierarchy Statistics** was used to identify the protocols present in the captured traffic and understand their relative contribution to the packet and byte count.

### Protocol Hierarchy

The observed protocol structure was:

```text
Ethernet
│
├── PPPoE
│   └── PPP
│       ├── LCP
│       ├── IPv6CP
│       ├── IPCP
│       └── CHAP
│
└── IPv4
    │
    ├── UDP
    │   ├── DNS / NTP / SIP / Syslog
    │   └── L2TP
    │       └── PPP
    │
    ├── TCP
    │   └── HTTP
    │       └── XML
    │
    ├── DHCP
    ├── DHCPv6
    ├── ICMP
    └── IGMP

IPv6
└── ICMPv6

ARP
|

Protocol Hierarchy 
| Protocol                                                  | % Packets | Packets | % Bytes |  Bytes |
| --------------------------------------------------------- | --------: | ------: | ------: | -----: |
| **Frame**                                                 |    100.0% |     531 |  100.0% |  7,862 |
| └── Ethernet                                              |    100.0% |     531 |   11.4% |  8,964 |
|   └── PPP-over-Ethernet Session                           |     50.1% |     266 |    2.0% |  1,596 |
|     └── Point-to-Point Protocol                           |     50.1% |     266 |    0.7% |    532 |
|       ├── PPP Link Control Protocol                       |      6.8% |      36 |    0.5% |    384 |
|       ├── PPP IPv6 Control Protocol                       |      0.4% |       2 |    0.0% |     28 |
|       ├── PPP IP Control Protocol                         |      2.3% |      12 |    0.3% |    216 |
|       └── PPP Challenge Handshake Authentication Protocol |      1.1% |       6 |    0.3% |    236 |
|   └── PPP-over-Ethernet Discovery                         |      3.0% |      16 |    1.1% |    904 |
| └── Internet Protocol Version 4                           |     69.7% |     370 |    9.4% |  7,412 |
|   └── User Datagram Protocol                              |     46.9% |     249 |    2.6% |  2,024 |
|     ├── Syslog Message                                    |      0.4% |       2 |    1.2% |    982 |
|     ├── Session Initiation Protocol                       |      0.8% |       4 |    3.7% |  2,890 |
|     ├── Network Time Protocol                             |      6.4% |      34 |    2.1% |  1,632 |
|     └── Layer 2 Tunneling Protocol                        |     16.2% |      86 |    1.3% |  1,005 |
|       └── Point-to-Point Protocol                         |     13.9% |      74 |    0.4% |    296 |
|         ├── PPP Password Authentication Protocol          |      0.4% |       2 |    0.1% |     57 |
|         ├── PPP Link Control Protocol                     |     10.7% |      57 |    0.8% |    668 |
|         ├── PPP IPv6 Control Protocol                     |      0.8% |       4 |    0.1% |     56 |
|         └── PPP IP Control Protocol                       |      0.2% |       1 |    0.0% |     22 |
|         └── Internet Protocol Version 6                   |      1.9% |      10 |    0.5% |    432 |
|           └── Internet Control Message Protocol v6        |      1.1% |       6 |    0.2% |    160 |
|   ├── Dynamic Host Configuration Protocol                 |      2.1% |      11 |    5.4% |  4,273 |
|   ├── Domain Name System                                  |     21.1% |     112 |    8.7% |  6,873 |
|   ├── DHCPv6                                              |      0.8% |       4 |    0.5% |    403 |
|   └── Transmission Control Protocol                       |     21.8% |     116 |    4.9% |  3,840 |
|     └── Hypertext Transfer Protocol                       |      7.3% |      39 |   29.2% | 22,929 |
|       ├── Unreassembled Fragmented Packet                 |      0.4% |       2 |    0.0% |      0 |
|       └── eXtensible Markup Language                      |      1.1% |       6 |    8.1% |  6,403 |
| ├── Internet Group Management Protocol                    |      0.6% |       3 |    0.1% |     24 |
| ├── Internet Control Message Protocol                     |      0.4% |       2 |    0.2% |    128 |
| └── Address Resolution Protocol                           |     16.8% |      89 |    3.2% |  2,492 |



