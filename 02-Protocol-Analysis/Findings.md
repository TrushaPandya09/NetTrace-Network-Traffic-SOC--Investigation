# 🔎 Investigation 02 — Protocol Analysis Findings

## 1. Investigation Summary

Protocol analysis was performed using **Wireshark → Statistics → Protocol Hierarchy** to identify the protocols present in the captured network traffic and understand their hierarchical relationships.

The capture contains **531 frames** with multiple protocol layers, including Ethernet, PPPoE, PPP, IPv4, IPv6, TCP, UDP, DNS, HTTP, ARP, DHCP, L2TP, NTP, SIP, ICMP, ICMPv6, IGMP, and other supporting protocols.

The protocol hierarchy was used to establish a baseline for subsequent TCP/UDP, application-layer, and security-focused investigations.

---

# 2. 🔬 Protocol Hierarchy Findings

The observed protocol hierarchy is:

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
        │   ├── Session Initiation Protocol
        │   ├── Network Time Protocol
        │   └── Layer 2 Tunneling Protocol
        │       └── Point-to-Point Protocol
        │           ├── PPP Password Authentication Protocol
        │           ├── PPP Link Control Protocol
        │           ├── PPP IPv6 Control Protocol
        │           └── PPP IP Control Protocol
        │
        ├── Dynamic Host Configuration Protocol
        ├── Domain Name System
        ├── DHCPv6
        │
        └── Transmission Control Protocol
            └── Hypertext Transfer Protocol
                ├── Unreassembled Fragmented Packet
                └── eXtensible Markup Language

    ├── Internet Protocol Version 6
    │   └── Internet Control Message Protocol v6
    │
    ├── Internet Group Management Protocol
    ├── Internet Control Message Protocol
    └── Address Resolution Protocol
4. 🔎 Key Findings
Finding 01 — IPv4 Is the Dominant Network-Layer Protocol

The capture contains:

370 IPv4 packets (69.7%)

IPv4 is the most prominent network-layer protocol observed in the Protocol Hierarchy.

Assessment

IPv4 carries several of the major protocols observed in the capture, including UDP and TCP.

This makes IPv4 traffic an important area for endpoint and communication analysis.

Finding 02 — UDP Traffic Is More Prominent Than TCP

The capture contains:

UDP: 249 packets (46.9%)
TCP: 116 packets (21.8%)

UDP therefore has a higher packet count than TCP in this capture.

UDP traffic includes:

DNS
NTP
SIP
Syslog
L2TP
Assessment

The significant UDP presence indicates that multiple UDP-based services are active within the captured traffic.

Further analysis should examine the individual UDP conversations, ports, endpoints, and application protocols.

Finding 03 — Significant DNS Activity

The capture contains:

112 DNS packets (21.1%)

Assessment

DNS represents a significant portion of the observed traffic.

DNS traffic should be examined further to identify:

Queried domains
Query types
DNS responses
Source and destination hosts
Repeated queries
Unusual DNS behavior

The protocol count alone does not indicate malicious DNS activity.

Finding 04 — HTTP Traffic Is Present

The capture contains:

39 HTTP packets (7.3%)

The HTTP traffic includes:

6 XML packets (1.1%)

The Protocol Hierarchy also identifies:

2 unreassembled fragmented packets

Assessment

HTTP traffic provides an opportunity for application-layer investigation.

Further analysis should examine:

HTTP methods
Host headers
Requested resources
Response codes
User-Agent information
HTTP streams
XML payloads
TCP connection behavior
Finding 05 — ARP Traffic Is Present

The capture contains:

89 ARP packets (16.8%)

Assessment

ARP traffic represents address-resolution activity within the captured network.

Further analysis can examine:

ARP requests
ARP replies
IP-to-MAC mappings
Repeated ARP requests
Unexpected mappings

The presence of ARP traffic alone does not establish malicious activity.

Finding 06 — PPPoE and PPP Traffic Are Significant

The capture contains:

PPPoE Session: 266 packets (50.1%)
PPPoE Discovery: 16 packets (3.0%)
PPP: 266 packets (50.1%)

PPP-related protocols include:

Protocol	Packets
PPP LCP	36
PPP IPv6CP	2
PPP IPCP	12
PPP CHAP	6
Assessment

The capture contains significant PPPoE and PPP-related traffic.

The hierarchy shows both session and discovery traffic along with PPP control and authentication protocols.

Finding 07 — L2TP Tunneling Traffic Is Present

The capture contains:

86 L2TP packets (16.2%)

Within the L2TP traffic, nested PPP traffic includes:

Protocol	Packets
PPP	74
PAP	2
LCP	57
IPv6CP	4
IPCP	1
Assessment

The presence of L2TP indicates tunneling-related traffic within the capture.

The nested PPP structure demonstrates that the capture contains multiple layers of encapsulated communication.

Further L2TP analysis can examine tunnel endpoints, sessions, and packet behavior.

Finding 08 — IPv6 and ICMPv6 Traffic Are Present

The capture contains:

IPv6: 10 packets (1.9%)
ICMPv6: 6 packets (1.1%)
Assessment

IPv6 represents a smaller portion of the captured traffic compared with IPv4.

ICMPv6 traffic indicates IPv6 control or diagnostic communication.

Finding 09 — DHCP and DHCPv6 Traffic Are Present

The capture contains:

DHCP: 11 packets (2.1%)
DHCPv6: 4 packets (0.8%)
Assessment

The presence of DHCP and DHCPv6 indicates address-configuration related traffic.

These packets may provide useful context for understanding host and network configuration.

Finding 10 — Additional Network Services Are Present

The capture also contains:

Protocol	Packets
NTP	34
SIP	4
IGMP	3
ICMP	2
Syslog	2
Assessment

These protocols represent additional network services and control traffic within the capture.

Their presence alone does not indicate malicious activity.

5. ❓ Investigation Questions & Answers
Q1. What protocols are present in the capture?

Answer: The capture contains Ethernet, PPPoE, PPP, IPv4, IPv6, TCP, UDP, DNS, HTTP, ARP, DHCP, DHCPv6, L2TP, NTP, SIP, ICMP, ICMPv6, IGMP, Syslog, PAP, CHAP, XML, and related protocols.

Q2. Which network-layer protocol is most prominent?

Answer: IPv4 is the most prominent network-layer protocol with 370 packets (69.7%).

Q3. Which transport protocol has more observed packets: TCP or UDP?

Answer: UDP has more observed packets.

Protocol	Packets
UDP	249
TCP	116

UDP therefore has 133 more packets than TCP.

Q4. Is DNS traffic present?

Answer: Yes. The capture contains 112 DNS packets (21.1%).

Q5. Is HTTP traffic present?

Answer: Yes. The capture contains 39 HTTP packets (7.3%). XML traffic is also observed within the HTTP hierarchy.

Q6. Is ARP traffic present?

Answer: Yes. The capture contains 89 ARP packets (16.8%).

Q7. Is IPv6 traffic present?

Answer: Yes. The capture contains 10 IPv6 packets (1.9%), including 6 ICMPv6 packets (1.1%).

Q8. Is PPPoE traffic present?

Answer: Yes. The capture contains 266 PPPoE Session packets (50.1%) and 16 PPPoE Discovery packets (3.0%).

Q9. Is tunneling traffic present?

Answer: Yes. 86 L2TP packets (16.2%) are present, with nested PPP traffic identified within the L2TP hierarchy.

Q10. Does the protocol hierarchy alone indicate malicious activity?

Answer: No.

The Protocol Hierarchy identifies the protocols and their distribution but does not provide sufficient evidence to classify the traffic as malicious.

Further analysis of:

Endpoints
Ports
TCP/UDP conversations
DNS queries
HTTP requests
Packet contents
Traffic patterns
Timing
Network streams

is required before making a security determination.

6. 🧠 Analyst Assessment
Overall Status: 🟢 Protocol Baseline Established

The Protocol Hierarchy analysis successfully established the protocol-level baseline of the PCAP.

The capture contains diverse traffic across multiple networking layers.

The most prominent observed protocols include:

IPv4
PPPoE
UDP
TCP
DNS
ARP
L2TP
HTTP

The capture also contains multiple supporting protocols including DHCP, DHCPv6, NTP, SIP, ICMP, ICMPv6, IGMP, Syslog, PAP, and CHAP.

Security Assessment

No malicious activity is confirmed from the Protocol Hierarchy alone.

The protocol analysis identifies areas that require deeper investigation but does not independently establish malicious behavior.


7. ✅ Conclusion

Investigation #2 established the protocol-level structure and distribution of the captured network traffic.

The PCAP contains 531 frames and multiple protocol layers. IPv4 is the dominant network-layer protocol, while UDP has a higher packet count than TCP.

Significant activity was observed for:

IPv4 — 370 packets
PPPoE Session — 266 packets
UDP — 249 packets
TCP — 116 packets
DNS — 112 packets
ARP — 89 packets
L2TP — 86 packets
HTTP — 39 packets

The protocol hierarchy also identified tunneling, authentication, network-management, and application-layer protocols.

These findings establish a reliable baseline for deeper TCP/UDP, DNS, HTTP, endpoint, and security analysis.

Final Assessment: The capture contains diverse network traffic across multiple protocol layers. Protocol identification alone does not indicate malicious activity; additional packet-level and contextual analysis is required.
