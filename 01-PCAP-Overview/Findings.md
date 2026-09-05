# 🔎 Investigation 01 — PCAP Overview Findings

## 1. Investigation Summary

The initial PCAP assessment was performed to establish a baseline understanding of the captured network traffic before conducting protocol-specific and endpoint-level investigations.

The capture contains multiple network protocols and communication between multiple hosts, providing a foundation for deeper traffic analysis.

---

## 2. Findings

### Finding 01 — Capture Characteristics

The analyzed capture was identified as:

| Attribute      | Finding            |
| -------------- | ------------------ |
| Capture File   | `nb6-startup.pcap` |
| File Format    | PCAP               |
| Total Packets  | 531                |
| File Size      | ~87 KB             |
| Captured Bytes | 78,623             |
| Encapsulation  | Ethernet           |

### Assessment

The capture contains sufficient packet data for further protocol, endpoint, and communication analysis.

---

### Finding 02 — Multiple Network Protocols

The initial review indicates that the PCAP contains **multiple network protocols** rather than a single type of network communication.

### Assessment

This makes the capture suitable for subsequent protocol hierarchy and traffic analysis.

The specific protocols and their distribution are examined in **Investigation 02 — Protocol Analysis**.

---

### Finding 03 — Multiple Hosts

Communication between **multiple hosts** was observed within the capture.

### Assessment

The presence of multiple communicating hosts provides the basis for identifying endpoints and analyzing host-to-host conversations.

This is examined further in **Investigation 03 — Host & Endpoint Analysis**.

---

### Finding 04 — Timestamp Anomaly

The capture reports an **unusually large interval between the first and last packet timestamps**.

### Assessment

This was documented as an initial anomaly in the capture metadata.

The timestamp range should be considered when performing chronological or timeline-based traffic analysis.

> **Important:** The timestamp anomaly alone is not sufficient to classify the traffic as malicious.

---

# 3. ❓ Investigation Questions & Answers

### Q1. What is the size of the capture?

**Answer:** The capture is approximately **87 KB** in size, with **78,623 captured bytes**.

---

### Q2. How many packets are present?

**Answer:** The capture contains **531 packets**.

---

### Q3. What encapsulation is used?

**Answer:** The capture uses **Ethernet encapsulation**.

---

### Q4. Does the capture contain multiple types of network traffic?

**Answer:** Yes. The initial assessment identified **multiple network protocols and communication between multiple hosts**.

---

### Q5. Is there any initial anomaly?

**Answer:** Yes. An **unusually large interval between the first and last packet timestamps** was observed.

---

### Q6. Does the timestamp anomaly confirm malicious activity?

**Answer:** No. The available PCAP overview information is insufficient to establish malicious activity. The anomaly should instead be considered during subsequent timeline and traffic analysis.

---


# 4. 🔗 Investigation Impact

The findings from this initial assessment establish the baseline for the next investigations:

```text
PCAP Overview
      ↓
Protocol Analysis
      ↓
Host & Endpoint Analysis
      ↓
TCP/UDP Analysis
      ↓
Application & Security Analysis
```

The timestamp observation should also be considered when performing future chronological traffic reconstruction.

---

# 5. 🧠 Analyst Assessment

### Overall Assessment


The initial assessment successfully established the basic characteristics and scope of the PCAP.

Multiple protocols and communicating hosts were identified, while an unusual timestamp interval was documented for consideration during further analysis.

No malicious activity is concluded from the PCAP overview alone.

---

# 7. ✅ Conclusion

The PCAP overview established the baseline required for the NetTrace investigation.

The capture contains **531 packets**, uses **Ethernet encapsulation**, and includes multiple protocols and communicating hosts. An unusually large timestamp interval was also identified and documented.

Further protocol and endpoint investigations are required to determine the nature and security relevance of the observed network communications.

