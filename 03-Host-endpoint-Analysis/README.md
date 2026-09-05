# 🔍 Host & Endpoint Analysis
# 🔍 Investigation 03 — Host & Endpoint Analysis

## 📌 Overview

This investigation analyzes the hosts and network endpoints observed in the `nb6-startup.pcap` capture using **Wireshark**.

The analysis focuses on identifying IPv4/IPv6 endpoints, examining endpoint traffic, identifying important TCP/UDP communications, and prioritizing hosts or connections for deeper investigation.

> **Note:** Detailed observations, answers, and analyst assessment are documented in [`findings.md`](./findings.md).

---

## 🎯 Objectives

- Identify active network endpoints.
- Analyze IPv4 and IPv6 hosts.
- Examine TCP and UDP endpoint activity.
- Identify high-volume communication.
- Identify internal-to-external communication.
- Determine connections requiring further investigation.
- Establish targets for the next TCP/UDP investigation.

---

## 🛠️ Tools & Data

| Component | Details |
|---|---|
| **Tool** | Wireshark |
| **Capture** | `nb6-startup.pcap` |
| **Analysis** | Endpoints |
| **Focus** | IPv4, IPv6, TCP, UDP and Ethernet endpoints |

---

## 1. 🌐 IPv4 Endpoint Analysis

**Wireshark → Statistics → Endpoints → IPv4**

The capture contains **18 IPv4 endpoints**.

Key endpoints selected for investigation include:

| Endpoint | Classification | Packets | Bytes |
|---|---|---:|---:|
| `10.251.23.139` | Internal | 152 | 44 KB |
| `10.194.143.1` | Internal | 3 | 1 KB |
| `172.26.235.86` | Private | 4 | 3 KB |
| `86.66.0.227` | External | 116 | 37 KB |
| `95.136.242.99` | External | 154 | 15 KB |
| `109.0.66.10` | External | 112 | 12 KB |
| `109.0.66.31` | External | 20 | 2 KB |
| `109.0.66.1` | External | 2 | 180 bytes |
| `109.6.1.72` | External | 86 | 7 KB |

> **Note:** The table contains selected endpoints relevant to the investigation. The complete IPv4 endpoint list is available in the Wireshark evidence screenshot.

---

## 2. 🌍 IPv6 Endpoint Analysis

**Wireshark → Statistics → Endpoints → IPv6**

The capture contains **5 IPv6 endpoints**.

Observed IPv6 endpoints include:

- `fe80::ca4c:75ff:fe78:ed00`
- `fe80::e2a1:d7ff:fe18:c270`
- `ff02::1`
- `ff02::16`
- `ff02::1:2`

IPv6 traffic was observed in addition to the IPv4 communications.

---

## 3. 🔗 TCP Endpoint Analysis

**Wireshark → Statistics → Endpoints → TCP**

The TCP endpoint analysis identified **9 TCP endpoint/port entries**.


## 4. 📡 UDP Endpoint Analysis

Wireshark → Statistics → Endpoints → UDP

The capture contains 87 UDP endpoint/port entries.

The UDP endpoint view shows multiple source and destination ports, indicating a variety of UDP-based communications within the capture.

These endpoints should be correlated with the protocol hierarchy and packet-level analysis performed in subsequent investigations.

## 5. 🖥️ Ethernet Endpoint Analysis

Wireshark → Statistics → Endpoints → Ethernet

The Ethernet endpoint view contains 87 endpoints.

The capture includes Ethernet-level communication in addition to the IPv4, IPv6, TCP and UDP endpoint activity.
## 6. 🚩 Priority Communication

The primary communication selected for further investigation is:

10.251.23.139
       ↓
86.66.0.227:80

Observed:

Protocol: TCP
Destination Port: 80
Packets: 116
Traffic: 37 KB

This communication was prioritized because it represents significant traffic between an internal endpoint and an external destination.

## 8. ❓ Investigation Questions

The answers are documented in findings.md.

1.How many IPv4 endpoints are present?
2.How many IPv6 endpoints are present?
3.Which internal endpoint has significant traffic?
4.Which external endpoints have significant traffic?
5.Which TCP communication should be prioritized?
6.What destination port is associated with the prioritized TCP communication?
7.How many packets and bytes were exchanged?
8.How many TCP and UDP endpoint entries are present?
9.Does external communication automatically indicate malicious activity?
10.What should be investigated next?

## Investigation Workflow


```text
10.251.23.139 → 86.66.0.227:80

