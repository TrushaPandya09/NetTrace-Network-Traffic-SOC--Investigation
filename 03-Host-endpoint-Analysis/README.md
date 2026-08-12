# 🔍 Host & Endpoint Analysis

## 📌 Overview

This analysis uses **Wireshark** to identify hosts, examine network conversations, and investigate potentially suspicious communication within the `nb6-startup.pcap` capture.

The analysis focuses on distinguishing **internal and external hosts** and identifying communication patterns that require further investigation.

---

## 🛠️ Tools & Data

* **Tool:** Wireshark
* **Capture:** `nb6-startup.pcap`
* **Analysis:** Endpoints & IPv4 Conversations

---

## 1. Endpoint Identification

**Wireshark → Statistics → Endpoints → IPv4**

The endpoint analysis was used to identify active hosts and classify them based on their IP addresses.

| Endpoint        | Type     | Packets | Bytes | Priority       |
| --------------- | -------- | ------: | ----: | -------------- |
| `10.251.23.139` | Internal |     152 | 44 KB | 🔴 High        |
| `10.194.143.1`  | Internal |       3 |  1 KB | 🟢 Low         |
| `172.26.235.86` | Private  |       4 |  3 KB | 🟢 Low         |
| `95.136.242.99` | External |     154 | 15 KB | 🟡 Investigate |
| `86.66.0.227`   | External |     116 | 37 KB | 🟡 Investigate |
| `109.0.66.10`   | External |     112 | 12 KB | 🟡 Investigate |

> **Note:** Public IP addresses are not automatically malicious. Traffic volume and external communication are indicators for further investigation

`Endpoints → IPv4`

---

## 2. Conversation Analysis

**Wireshark → Statistics → Conversations → IPv4**

The conversation view was used to determine which hosts communicated with each other and how much traffic was exchanged.

### Key Communication

| Source          | Destination   | Packets | Bytes | Observation                     |
| --------------- | ------------- | ------: | ----: | ------------------------------- |
| `10.251.23.139` | `86.66.0.227` |     116 | 37 KB | **High-priority investigation** |
| `10.251.23.139` | `109.0.66.31` |      20 |  2 KB | Investigate                     |
| `10.251.23.139` | `109.0.66.1`  |       2 | 180 B | Low activity                    |
| `10.251.23.139` | `109.0.66.10` |       2 | 192 B | Low activity                    |


`Conversations → IPv4`

---

## 🚩 3. Suspicious Activity Assessment

The communication:

```text
10.251.23.139 → 86.66.0.227
```

was prioritized because an internal endpoint exchanged **116 packets / 37 KB** with an external IP.

This does **not confirm malicious activity**. Further investigation is required by checking:

* Source and destination ports
* Protocol
* DNS/domain information
* Packet contents
* Communication timing
* Repeated connections
* Threat intelligence for the destination IP

### Initial Assessment

**Status:** 🟡 **Requires Further Investigation**

---

## 🧠 Investigation Workflow

```text
Identify Endpoints
       ↓
Classify Internal / External
       ↓
Analyze Conversations
       ↓
Identify High-Priority Connections
       ↓
Check Ports & Protocols
       ↓
Inspect Packets
       ↓
Enrich IOCs
       ↓
Final Assessment
```

---

## 🎯 Key Takeaways

* Identified internal and external IPv4 endpoints.
* Analyzed host-to-host communication using IPv4 Conversations.
* Prioritized high-volume internal-to-external communication.
* Avoided treating public IPs or high traffic as automatically malicious.
* Established further investigation steps for suspicious connections.

---

## ⚠️ Disclaimer

This analysis was performed for **educational and authorized cybersecurity research purposes** using the provided PCAP file.

