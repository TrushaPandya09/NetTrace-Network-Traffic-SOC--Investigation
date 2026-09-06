# Investigation 04 — TCP & UDP Analysis

## 📌 Investigation Overview

This investigation analyzes TCP and UDP communication within the `nb6-startup.pcap` capture using Wireshark.

The objective is to understand transport-layer behavior, validate TCP connection establishment and data transfer, examine UDP communication patterns, and identify any observable protocol anomalies.

The investigation focuses on:

- TCP connection establishment
- TCP header and flag analysis
- TCP data transfer
- TCP stream reconstruction
- TCP reset behavior
- UDP traffic overview
- UDP header analysis
- UDP-based application traffic
- DNS query and response behavior
- Basic transport-layer anomaly checks

---

## 🎯 Objectives

The primary objectives of this investigation are:

1. Analyze TCP connection establishment using the three-way handshake.
2. Examine TCP header fields and control flags.
3. Identify TCP packets carrying application-layer data.
4. Reconstruct TCP application traffic using Follow TCP Stream.
5. Examine TCP reset behavior where present.
6. Analyze UDP communication and identify major UDP-based protocols.
7. Inspect UDP header fields including ports, length, and checksum.
8. Analyze DNS traffic carried over UDP.
9. Check for common TCP/UDP transport anomalies.
10. Document findings using packet-level evidence.

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | Packet capture analysis and protocol inspection |
| Wireshark Display Filters | Traffic isolation and anomaly detection |
| Follow TCP Stream | TCP application-layer stream reconstruction |

---

## 📂 Evidence Source

**PCAP:** `nb6-startup.pcap`

**Primary Analysis Tool:** Wireshark

**Investigation:** TCP & UDP Transport-Layer Analysis

---

# 1. TCP Analysis

## 1.1 TCP Traffic Identification

TCP traffic was isolated using the Wireshark display filter:

```text
tcp
````

Representative TCP packets were selected to examine connection establishment,
TCP headers, flags, sequence/acknowledgment numbers, and application data.

## 2. TCP Three-Way Handshake

A TCP connection was analyzed by following the packet sequence:

SYN → SYN/ACK → ACK

This was used to verify successful TCP connection establishment.

## 3. TCP Header & Flag Analysis

A representative TCP packet was inspected by expanding the
Transmission Control Protocol section in Wireshark.

The following fields were examined:

Source Port
Destination Port
Sequence Number
Acknowledgment Number
Header Length
TCP Flags
Window Size
Checksum
TCP Options
## 4. TCP Data Transfer

A packet carrying TCP payload data was examined to determine whether data was
transferred after the handshake.

Sequence numbers, acknowledgment numbers, segment length, and PSH/ACK flags
were analyzed.

## 5. TCP Application Traffic

Follow → TCP Stream was used to reconstruct the application-layer
conversation carried over TCP.

This was used to identify the application protocol and inspect readable
application data.

## 6. TCP Reset & Anomaly Analysis

TCP traffic was reviewed for reset activity and common TCP analysis indicators
such as retransmissions and other abnormal packet behavior.

## 7. UDP Traffic Analysis

UDP traffic was isolated using:

udp

Representative DHCP and DNS packets were selected for analysis.

## 8. UDP Header Analysis

The User Datagram Protocol section was expanded to inspect:

Source Port
Destination Port
Length
Checksum
## 9. UDP Application Traffic

DNS traffic was isolated using:

dns

A DNS query and corresponding response were examined using the DNS transaction
ID, queried hostname, and query type to correlate the request and response.

## Evidence Collected

TCP Three-Way Handshake	Verify TCP connection establishment

TCP Header / SYN-ACK	Analyze TCP fields and flags

TCP Data Transfer	Demonstrate TCP payload transmission

Follow TCP Stream	Examine TCP application-layer data

TCP RST/ACK	Document observed reset activity

UDP Overview / DHCP	Demonstrate UDP traffic

UDP Header	Analyze UDP transport fields

DNS Query & Response	Demonstrate UDP application-layer traffic

## Investigation Outcome

The analysis demonstrated both connection-oriented TCP communication and
connectionless UDP communication.

TCP traffic showed a complete handshake, application data transfer, HTTP
traffic, and an observed RST/ACK packet. UDP traffic included DHCP and DNS,
with a DNS query and corresponding response identified.

The detailed observations and answers to the investigation questions are
documented in findings.md.

```
