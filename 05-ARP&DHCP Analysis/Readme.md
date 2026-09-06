# Investigation 04 — ARP & DHCP Analysis

## 📌 Investigation Overview

This investigation analyzes **ARP (Address Resolution Protocol)** and **DHCP (Dynamic Host Configuration Protocol)** traffic within the `nb6-startup.pcap` capture using Wireshark.

The objective is to understand Layer 2 address resolution and dynamic IP configuration behavior, identify normal ARP request/reply communication, and analyze the DHCP DORA process used for network configuration.

The investigation focuses on:

- ARP request and reply analysis
- IP-to-MAC address resolution
- ARP packet structure and fields
- DHCP Discover analysis
- DHCP Request analysis
- DHCP ACK analysis
- DHCP client and server identification
- DHCP configuration parameters
- Identification of unusual or repeated address-resolution behavior

---

## 🎯 Objectives

The primary objectives of this investigation are:

1. Identify ARP traffic within the packet capture.
2. Analyze ARP request packets.
3. Analyze corresponding ARP reply packets.
4. Determine the IP-to-MAC address resolution process.
5. Examine ARP packet fields including opcode, sender and target addresses.
6. Identify DHCP traffic within the capture.
7. Analyze the DHCP Discover message.
8. Identify the DHCP Offer, Request, and ACK messages.
9. Validate the DHCP DORA process.
10. Identify the client IP, DHCP server, relay, gateway, subnet mask, and DNS configuration.
11. Identify repeated DHCP attempts and determine whether they represent abnormal behavior.
12. Document findings using packet-level evidence.

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Wireshark | Packet capture analysis and protocol inspection |
| Wireshark Display Filters | ARP and DHCP traffic isolation |
| `arp` filter | Identify ARP traffic |
| `arp.opcode == 2` | Identify ARP replies |
| `dhcp` filter | Identify DHCP traffic |

---

## 📂 Evidence Source

**PCAP:** `nb6-startup.pcap`

**Primary Analysis Tool:** Wireshark

**Protocols Analyzed:**
- ARP
- DHCP
- UDP

---

# 1. ARP Analysis

## 1.1 ARP Traffic Identification

ARP traffic was isolated using the Wireshark display filter:

```text
arp
```
The filtered traffic contained multiple ARP requests used to resolve IPv4 addresses to corresponding MAC addresses.

The packet list showed requests such as:

Who has 10.251.196.227? Tell 10.251.196.1

This indicates that the sender was attempting to determine the MAC address associated with the target IP address.

## 1.2 ARP Request Analysis

A representative ARP request was inspected by expanding the:

Address Resolution Protocol (request)

section in Wireshark.

The selected request contained:

Hardware Type: Ethernet
Protocol Type: IPv4
Hardware Address Length: 6
Protocol Address Length: 4
Opcode: Request (1)
Sender IP: 10.251.196.1
Sender MAC: 80:fb:06:f0:45:d7
Target IP: 10.251.196.227
Target MAC: 00:00:00:00:00:00

The zeroed target MAC is expected in an ARP request because the sender is attempting to discover the target's MAC address.

## 1.3 ARP Reply Analysis

ARP replies were isolated using:

arp.opcode == 2

The selected ARP reply contained:

Opcode: Reply (2)
Sender IP: 10.251.23.1
Sender MAC: 80:fb:06:f0:45:d7
Target IP: 10.251.23.139
Target MAC: e0:a1:d7:18:c2:72

The packet information indicated:

10.251.23.1 is at 80:fb:06:f0:45:d7

This demonstrates successful IP-to-MAC address resolution.


# 2. DHCP Analysis
## 2.1 DHCP Traffic Identification

DHCP traffic was isolated using:

dhcp

The capture contained multiple DHCP Discover messages followed by DHCP Offer, Request, and ACK messages.

This allowed the DHCP address assignment process to be reconstructed.

## 2.2 DHCP Discover

A representative DHCP Discover packet was examined.

The packet contained:

Source IP: 0.0.0.0
Destination IP: 255.255.255.255
Source Port: 68
Destination Port: 67
DHCP Message Type: Discover
Transaction ID: 0x0a068aaf
Client MAC: e0:a1:d7:18:c2:72

The client used 0.0.0.0 because it had not yet received an IP address.

The broadcast destination 255.255.255.255 allows the DHCP Discover message to reach available DHCP servers.

## 2.3 DHCP DORA Process

The capture contained the four stages of the DHCP address assignment process:

DHCP Discover
      ↓
DHCP Offer
      ↓
DHCP Request
      ↓
DHCP ACK

The observed packets included:

Packet	DHCP Message	Direction
57	Discover	0.0.0.0 → 255.255.255.255
59	Offer	10.194.143.1 → 10.251.23.139
60	Request	0.0.0.0 → 255.255.255.255
61	ACK	10.194.143.1 → 10.251.23.139

The sequence demonstrates a successful DHCP configuration exchange.

## 2.4 DHCP Configuration Details

The DHCP ACK packet was examined to identify the configuration provided to the client.

The packet contained:

DHCP Message Type: ACK
Transaction ID: 0x0a068aaf
Assigned Client IP: 10.251.23.139
DHCP Server Identifier: 86.64.145.166
Relay Agent IP: 10.194.143.1
Client MAC: e0:a1:d7:18:c2:72
Subnet Mask: 255.255.255.0
Router information
DNS Server information
IP Address Lease Time

The DHCP ACK confirms that the client successfully received network configuration.

# 3. Security & Anomaly Review

The ARP traffic was reviewed for basic indicators such as:

Excessive ARP requests
Suspicious ARP replies
Unexpected MAC address mappings
Repeated address resolution
Potential ARP spoofing indicators

The observed ARP request/reply traffic demonstrated normal address-resolution behavior.

The DHCP traffic contained multiple Discover attempts before a successful DORA exchange. Repeated Discover messages can occur when a client retries while waiting for a DHCP response and were not treated as malicious based solely on the observed packets.

No definitive ARP spoofing or DHCP-based attack was established from the reviewed evidence.

## 4. Evidence Collected
Evidence	Purpose
ARP Overview	Identify ARP traffic and requests

ARP Reply	Demonstrate IP-to-MAC resolution

DHCP Discover	Identify DHCP client discovery

DHCP Configuration Details	Analyze DHCP ACK and assigned configuration

DHCP DORA sequence	Validate successful DHCP address assignment

# 5. Investigation Outcome

The ARP analysis demonstrated normal IPv4-to-MAC address resolution through ARP request and reply communication.

The DHCP analysis demonstrated a complete DHCP DORA exchange consisting of Discover, Offer, Request, and ACK messages.

The DHCP ACK provided the client with the IP address 10.251.23.139 along with subnet, router, DNS, DHCP server, and lease-related configuration.

The reviewed traffic did not provide sufficient evidence of ARP spoofing or DHCP-based malicious activity.

Detailed packet-level observations and security interpretation are documented in findings.md.
