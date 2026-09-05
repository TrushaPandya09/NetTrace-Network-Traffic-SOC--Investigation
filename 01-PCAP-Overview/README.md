# 🔍 Investigation 01 — PCAP Overview

## 📌 Overview

This investigation performs the initial assessment of the network capture before conducting detailed protocol, endpoint, and traffic analysis.

The purpose is to establish a baseline of the PCAP, understand its structure and scope, and identify observations that should be considered during subsequent investigations.

---

## 🎯 Objectives

* Identify the capture file and its basic characteristics.
* Determine the total number of captured packets.
* Identify the capture format and encapsulation.
* Review the capture duration and timestamp information.
* Establish an initial understanding of the network traffic.
* Identify observations that may require validation during deeper analysis.

---

## 🛠️ Tools Used

| Tool      | Purpose                                      |
| --------- | -------------------------------------------- |
| Wireshark | PCAP inspection and network traffic analysis |

---

## 📂 Capture Information

| Attribute      | Value              |
| -------------- | ------------------ |
| Capture File   | `nb6-startup.pcap` |
| File Format    | PCAP               |
| Packets        | 531                |
| File Size      | ~87 KB             |
| Captured Bytes | 78,623             |
| Encapsulation  | Ethernet           |

---

## 🔬 Investigation Methodology

### Step 1 — Load the PCAP

The capture file was opened in Wireshark to verify that the packet data was accessible and suitable for analysis.

---

### Step 2 — Review Capture Information

The capture information was reviewed to determine:

* Packet count
* File size
* Encapsulation type
* Captured bytes
* Timestamp range

---

### Step 3 — Perform Initial Traffic Review

The packet list was reviewed to obtain an initial understanding of the traffic contained within the capture.

This stage was intentionally limited to a high-level assessment. Detailed protocol, endpoint, conversation, and application analysis are covered in subsequent investigations.

---

## ❓ Investigation Questions

1. What is the size of the capture?
2. How many packets are present?
3. What encapsulation is used?
4. What is the capture's timestamp range?
5. Does the capture contain multiple types of network traffic?
6. Are there any initial observations requiring further investigation?

---

## 🔗 Investigation Scope

This investigation establishes the baseline for:

* Protocol Analysis
* Host & Endpoint Analysis
* TCP/UDP Analysis
* DNS Analysis
* HTTP Analysis
* TLS/HTTPS Analysis
* Conversation and timeline analysis

---

## 📄 Findings

The evidence-based observations and analyst assessment are documented separately in:

`findings.md`

---

## ⚠️ Disclaimer

This investigation was conducted for educational and authorized cybersecurity research purposes using the provided PCAP capture.


