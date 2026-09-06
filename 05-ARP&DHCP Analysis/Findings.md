# Investigation 04 — ARP & DHCP Analysis Findings

## 1. Investigation Summary

ARP and DHCP traffic within `nb6-startup.pcap` was analyzed using Wireshark.

The investigation examined:

- ARP request and reply behavior
- IP-to-MAC address resolution
- ARP packet fields
- DHCP Discover activity
- DHCP DORA sequence
- DHCP configuration parameters
- Repeated DHCP discovery behavior
- Basic indicators of ARP or DHCP-related anomalies

The analysis identified normal ARP address-resolution activity and a successful DHCP configuration exchange.

---

# 2. ARP Findings

## 2.1 ARP Traffic Identification

The following Wireshark display filter was used:

```text
arp
```
Multiple ARP packets were identified in the capture.

The packet list contained requests such as:

Who has 10.251.196.227? Tell 10.251.196.1

This indicates that a host was attempting to determine the MAC address associated with an IPv4 address.

Observation

The presence of ARP requests is expected in an IPv4 network because devices use ARP to resolve local IP addresses to Layer 2 MAC addresses.


## 2.2 ARP Request Details

A representative ARP request was inspected.

Packet Details
Field	Observed Value
Hardware Type	Ethernet
Protocol Type	IPv4
Hardware Address Length	6
Protocol Address Length	4
Opcode	Request (1)
Sender IP	10.251.196.1
Sender MAC	80:fb:06:f0:45:d7
Target IP	10.251.196.227
Target MAC	00:00:00:00:00:00
Interpretation

The sender 10.251.196.1 is attempting to discover the MAC address associated with 10.251.196.227.

The target MAC is all zeros because the sender does not yet know the target's MAC address.

This is consistent with a normal ARP request.

## 2.3 ARP Reply Details

ARP replies were isolated using:

arp.opcode == 2

The selected packet contained:

Field	Observed Value
Opcode	Reply (2)
Sender IP	10.251.23.1
Sender MAC	80:fb:06:f0:45:d7
Target IP	10.251.23.139
Target MAC	e0:a1:d7:18:c2:72

The packet information displayed:

10.251.23.1 is at 80:fb:06:f0:45:d7
Interpretation

The ARP reply provides the requesting host with the MAC address associated with 10.251.23.1.

This confirms successful IP-to-MAC address resolution.

# 3. DHCP Findings
## 3.1 DHCP Traffic Identification

DHCP traffic was isolated using:

dhcp

The capture contained multiple DHCP messages, including:

DHCP Discover
DHCP Offer
DHCP Request
DHCP ACK

This allowed the DHCP address-assignment sequence to be reconstructed.

# 4. DHCP Discover Analysis

A representative DHCP Discover packet was examined.

Packet Details
Field	Observed Value
Source IP	0.0.0.0
Destination IP	255.255.255.255
Source Port	68
Destination Port	67
DHCP Message Type	Discover
Transaction ID	0x0a068aaf
Client MAC	e0:a1:d7:18:c2:72
Interpretation

The client uses source IP 0.0.0.0 because it does not yet have an assigned IPv4 address.

The broadcast destination 255.255.255.255 allows the DHCP Discover message to reach DHCP servers on the local network.

The client is effectively asking:

"Is there a DHCP server available to provide network configuration?"

## 5. DHCP DORA Analysis

The capture contains the standard DHCP DORA sequence.

Discover → Offer → Request → ACK
Observed Sequence
Packet	Message	Source	Destination
57	DHCP Discover	0.0.0.0	255.255.255.255
59	DHCP Offer	10.194.143.1	10.251.23.139
60	DHCP Request	0.0.0.0	255.255.255.255
61	DHCP ACK	10.194.143.1	10.251.23.139
Interpretation

The observed sequence represents a successful DHCP address configuration process.

The client first broadcasts a Discover message, receives an Offer, requests the offered configuration, and finally receives an ACK from the DHCP infrastructure.

## 6. DHCP Configuration Details

The DHCP ACK packet was inspected in detail.

Observed Configuration
Parameter	Observed Value
DHCP Message Type	ACK
Transaction ID	0x0a068aaf
Client IP	10.251.23.139
DHCP Server Identifier	86.64.145.166
Relay Agent IP	10.194.143.1
Client MAC	e0:a1:d7:18:c2:72
Subnet Mask	255.255.255.0
Router	Present
DNS Server	Present
Lease Time	Present
Key Finding

The DHCP ACK confirms that the client successfully received the IPv4 address:

10.251.23.139

along with additional network configuration parameters.


## 7. Repeated DHCP Discover Activity

Multiple DHCP Discover packets were observed before the successful DORA exchange.

Examples in the capture include repeated Discover messages with different transaction IDs.

Interpretation

Repeated DHCP Discover messages can occur when a client does not receive a response within the expected period and retries the discovery process.

The observed retries were followed by a successful Offer, Request, and ACK sequence.

Therefore, the repeated Discover traffic alone is not sufficient to classify the behavior as malicious.

# 8. Security Analysis
## 8.1 ARP Security Review

The ARP traffic was reviewed for basic indicators associated with ARP spoofing or poisoning, including:

Unexpected IP-to-MAC mappings
Suspicious ARP replies
Excessive unsolicited replies
Conflicting MAC addresses for the same IP
Abnormal ARP behavior

The reviewed ARP request/reply traffic showed expected IP-to-MAC resolution behavior.

No definitive ARP spoofing activity was established from the reviewed evidence.

## 8.2 DHCP Security Review

The DHCP traffic was reviewed for:

Unexpected DHCP servers
Abnormal DHCP message sequences
Excessive DHCP requests
Repeated discovery attempts
Inconsistent DHCP configuration

A DHCP server identifier of:

86.64.145.166

was observed in the DHCP ACK.

The capture also showed a relay agent:

10.194.143.1

The DHCP exchange completed successfully through the standard DORA sequence.

No definitive evidence of DHCP starvation or rogue DHCP activity was established from the reviewed packets.

# 9. Investigation Questions & Answers
Q1. What is the purpose of ARP?

ARP resolves an IPv4 address to the corresponding MAC address on a local network.

Q2. What does an ARP Request contain?

An ARP Request contains the sender's IP/MAC information and the target IP whose MAC address is being requested.

Q3. What does an ARP Reply provide?

The ARP Reply provides the MAC address associated with the requested IP address.

Q4. What was the observed ARP reply?

The capture showed:

10.251.23.1 is at 80:fb:06:f0:45:d7
Q5. Why does DHCP Discover use 0.0.0.0?

The client has not yet received an IP address, so it uses 0.0.0.0 as the source address.

Q6. Why is DHCP Discover broadcast?

The client does not initially know the DHCP server's address, so it broadcasts the discovery message.

Q7. What are the four DHCP DORA stages?
Discover
Offer
Request
ACK
Q8. What IP address was assigned to the client?

The DHCP ACK indicates:

10.251.23.139
Q9. What DHCP server was identified?

The DHCP Server Identifier was:

86.64.145.166
Q10. Was suspicious ARP or DHCP activity confirmed?

No definitive ARP spoofing, DHCP starvation, or rogue DHCP activity was established from the reviewed evidence.

## 10. Evidence Summary
Evidence	Finding
ARP_Overview.png	Multiple ARP requests identified
ARP_Reply.png	Successful IP-to-MAC resolution observed
DHCP_Discover.png	Client broadcast DHCP discovery identified
DHCP_Configuration_Details.png	DHCP ACK and network configuration identified

## 11. Overall Findings

The investigation demonstrated normal ARP and DHCP activity within the analyzed PCAP.

ARP
ARP requests were observed for IPv4-to-MAC resolution.
ARP replies provided the corresponding MAC address information.
The analyzed request/reply behavior was consistent with normal network operation.
DHCP
Multiple DHCP Discover messages were observed.
A complete DORA sequence was identified.
The client successfully received 10.251.23.139.
DHCP configuration included subnet mask, router, DNS, server identifier, and lease information.
Repeated Discover messages were observed before successful configuration but were not independently indicative of malicious activity.
Security Assessment

No definitive evidence of ARP spoofing, DHCP starvation, or rogue DHCP behavior was established from the reviewed packet-level evidence.

## 12. Conclusion

The ARP and DHCP analysis successfully demonstrated two fundamental network services:

ARP provided Layer 2 address resolution between IPv4 addresses and MAC addresses, while DHCP provided dynamic network configuration through the standard Discover → Offer → Request → ACK process.

The investigation provides packet-level evidence of normal address resolution and successful client network configuration and forms part of the broader NetTrace network traffic investigation.
