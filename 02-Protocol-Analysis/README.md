# 02 — Protocol Analysis
# 🔬 Investigation 02 — Protocol Analysis

## 📌 Overview

This investigation analyzes the protocols observed within the `nb6-startup.pcap` capture using Wireshark.

The objective is to understand the protocol distribution, network-layer structure, transport protocols, and application-layer protocols present in the traffic.

---

## 🎯 Objectives

* Identify protocols present in the PCAP.
* Analyze the protocol hierarchy.
* Determine protocol distribution by packet and byte count.
* Identify major network and transport-layer protocols.
* Identify application-layer protocols.
* Understand protocol encapsulation and layering.
* Identify protocols that should be examined further in later investigations.

---

## 🛠️ Tool Used

| Tool      | Purpose                                                    |
| --------- | ---------------------------------------------------------- |
| Wireshark | Protocol hierarchy, packet inspection and traffic analysis |

---

# 1. 🔬 Protocol Hierarchy Analysis

Wireshark's **Statistics → Protocol Hierarchy** feature was used to identify the protocols present in the capture and understand their relative contribution to the traffic.

## Observed Protocol Structure

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
    │   ├── DNS
    │   ├── NTP
    │   ├── SIP
    │   ├── Syslog
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
│
IPv6
└── ICMPv6

ARP

---

# 2. 📊 Protocol Statistics

The following values were obtained from the Wireshark Protocol Hierarchy analysis.

| Protocol | % Packets | Packets | % Bytes |  Bytes |
| -------- | --------: | ------: | ------: | -----: |
| Ethernet |    100.0% |     531 |   11.4% |  8,964 |
| IPv4     |     69.7% |     370 |    9.4% |  7,412 |
| UDP      |     46.9% |     249 |    2.6% |  2,024 |
| TCP      |     21.8% |     116 |    4.9% |  3,840 |
| DNS      |     21.1% |     112 |    8.7% |  6,873 |
| ARP      |     16.8% |      89 |    3.2% |  2,492 |
| L2TP     |     16.2% |      86 |    1.3% |  1,005 |
| HTTP     |      7.3% |      39 |   29.2% | 22,929 |
| NTP      |      6.4% |      34 |    2.1% |  1,632 |
| DHCP     |      2.1% |      11 |    5.4% |  4,273 |
| ICMPv6   |      1.1% |       6 |    0.2% |    160 |

> The complete protocol hierarchy and statistics are retained as part of the investigation evidence.

---

# 3. 🌐 Network-Layer Protocols

The capture contains traffic associated with:

* IPv4
* IPv6
* ARP
* ICMP
* ICMPv6
* IGMP

These protocols provide the foundation for understanding addressing, routing, control, and local network communication within the capture.

---

# 4. 🚚 Transport-Layer Protocols

### TCP

TCP traffic was identified with **116 packets**.

TCP provides connection-oriented communication and will be examined further in the dedicated TCP/UDP investigation.

### UDP

UDP was identified with **249 packets**, making it a significant transport protocol within the capture.

UDP-based application traffic includes protocols such as DNS, NTP, SIP, Syslog, and L2TP.

---

# 5. 🧩 Application & Supporting Protocols

The capture contains several application and supporting protocols, including:

* DNS
* HTTP
* NTP
* SIP
* Syslog
* DHCP
* DHCPv6
* L2TP
* PPP
* PPPoE

These protocols provide different types of network services and will be examined according to their relevance in later investigations.

---

# 6. 🔐 Protocols Requiring Further Analysis

The protocol hierarchy identifies several areas for deeper investigation:

| Protocol | Follow-up Investigation                   |
| -------- | ----------------------------------------- |
| DNS      | Queries, domains, responses               |
| HTTP     | Requests, responses, URLs, streams        |
| TCP      | Handshakes, flags, ports, streams         |
| UDP      | Conversations and traffic patterns        |
| ARP      | Address resolution and host relationships |
| DHCP     | Host configuration activity               |
| L2TP/PPP | Tunneling and encapsulated traffic        |

---

# 7. ❓ Investigation Questions

1. Which protocols are present?
2. Which network-layer protocols dominate the capture?
3. How much TCP and UDP traffic is present?
4. Which application-layer protocols are visible?
5. What protocol relationships and encapsulation layers exist?
6. Which protocols require deeper investigation?

---


# 8. 🔗 Investigation Scope

The protocol analysis establishes the foundation for subsequent investigations involving:

* TCP/UDP communication
* DNS activity
* HTTP traffic
* TLS/HTTPS traffic
* Host and endpoint behavior
* Conversation analysis

---

# 📄 Findings

The evidence-based observations, security relevance, and analyst assessment are documented separately in:

`findings.md`

---

## ⚠️ Disclaimer

This investigation was conducted for educational and authorized cybersecurity research purposes using the provided PCAP capture.

