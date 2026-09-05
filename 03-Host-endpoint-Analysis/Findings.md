## 🔎 Investigation 03 — Host & Endpoint Analysis Findings
1. Investigation Summary

Host and endpoint analysis was performed on the nb6-startup.pcap capture using Wireshark's Statistics → Endpoints functionality.

The investigation examined Ethernet, IPv4, IPv6, TCP and UDP endpoints to identify active hosts, communication patterns, and connections requiring further analysis.

A significant TCP communication between an internal endpoint and an external destination was identified and selected as the primary target for further investigation.

## 2. 🌐 IPv4 Endpoint Findings

The IPv4 Endpoint view contains:

18 IPv4 endpoints

The following key endpoints were observed:

Endpoint	Classification	Packets	Bytes
10.251.23.139	Internal	152	44 KB
10.194.143.1	Internal	3	1 KB
172.26.235.86	Private	4	3 KB
86.66.0.227	External	116	37 KB
95.136.242.99	External	154	15 KB
109.0.66.10	External	112	12 KB
109.0.66.31	External	20	2 KB
109.0.66.1	External	2	180 bytes
109.6.1.72	External	86	7 KB

Additional IPv4 endpoints are present in the capture.

Assessment

The endpoint analysis shows communication involving internal/private addresses as well as multiple external IPv4 addresses.

The internal endpoint:

10.251.23.139

was observed with significant network activity.

## 3. 🌍 IPv6 Endpoint Findings

The IPv6 Endpoint view contains:

5 IPv6 endpoints

Observed addresses include:

fe80::ca4c:75ff:fe78:ed00
fe80::e2a1:d7ff:fe18:c270
ff02::1
ff02::16
ff02::1:2
Assessment

IPv6 communication is present in the capture alongside IPv4 traffic.

The observed IPv6 endpoints include link-local and multicast addresses.

For this investigation, the primary focus remains on the IPv4/TCP communication requiring deeper analysis.

## 4. 🔗 TCP Endpoint Findings

The TCP Endpoint view contains:

9 TCP endpoint/port entries

The most significant entry is:

10.251.23.139 → 86.66.0.227:80

Observed
Attribute	Value
Source	10.251.23.139
Destination	86.66.0.227
Destination Port	80
Protocol	TCP
Packets	116
Bytes	37 KB
Assessment

The communication was selected as the primary target for further investigation because it involves an internal endpoint communicating with an external destination and contains a relatively significant amount of observed traffic.

Port 80 identifies the destination service as HTTP traffic at the transport/application boundary.

Important: The presence of an external IP or TCP port 80 does not by itself indicate malicious activity.

## 5. 📡 UDP Endpoint Findings

The UDP Endpoint view contains:

87 UDP endpoint/port entries

The screenshot shows numerous UDP endpoint/port combinations involving different source and destination ports.

Assessment

The number of UDP endpoint entries indicates that UDP represents a significant part of the endpoint activity within the capture.

The individual UDP communications require correlation with the protocol hierarchy and packet contents to determine their purpose.

## 6. 🖥️ Ethernet Endpoint Findings

The Ethernet Endpoint view contains:

87 Ethernet endpoints

The endpoint list includes Ethernet-level addresses and associated packet/byte activity.

Assessment

The Ethernet endpoint analysis provides a lower-level view of the hosts and devices participating in the captured network traffic.

This supports the higher-layer IPv4, IPv6, TCP and UDP endpoint analysis.

## 7. 🚩 Finding 01 — Priority Internal-to-External TCP Communication

The primary communication identified for further investigation is:

10.251.23.139 → 86.66.0.227:80

Observed Traffic
116 packets
37 KB
TCP
Destination Port: 80
Why it was prioritized

The communication was prioritized because:

The source is an internal IPv4 endpoint.
The destination is an external IPv4 address.
The connection uses TCP.
The destination port is 80.
The communication contains 116 packets and approximately 37 KB of traffic.
Security Assessment

Status: 🟡 Requires Further Investigation

The available endpoint evidence does not establish malicious activity.

Further packet-level investigation is required.

## 8. 🚩 Finding 02 — Significant External Endpoints

Several external endpoints show notable packet activity.

Examples include:

External Endpoint	Packets	Bytes
95.136.242.99	154	15 KB
109.0.66.10	112	12 KB
86.66.0.227	116	37 KB
109.6.1.72	86	7 KB
Assessment

These endpoints are useful investigation targets because of their observed traffic levels.

However, traffic volume alone cannot determine whether an endpoint is benign or malicious.

Additional context such as protocol, port, DNS information, packet contents and connection behavior is required.

## 9. ❓ Investigation Questions & Answers
Q1. How many IPv4 endpoints are present?

Answer: The Wireshark IPv4 Endpoint view contains 18 IPv4 endpoints.

Q2. How many IPv6 endpoints are present?

Answer: The IPv6 Endpoint view contains 5 IPv6 endpoints.

Q3. Which internal endpoint has significant traffic?

Answer:

10.251.23.139

was observed with 152 packets and approximately 44 KB of traffic.

Q4. Which external endpoint has the highest packet count?

Answer:

95.136.242.99

was observed with 154 packets, the highest packet count among the listed IPv4 endpoints.

Q5. Which external endpoint has the highest traffic volume?

Answer:

86.66.0.227

was observed with approximately 37 KB, making it the highest-byte external endpoint among the listed endpoints.

Q6. Which TCP communication was prioritized?

Answer:

10.251.23.139 → 86.66.0.227:80

was selected as the primary communication for further investigation.

Q7. How many packets and bytes were exchanged?

Answer: The TCP endpoint view shows 116 packets and approximately 37 KB for the communication.

Q8. What destination port is being used?

Answer: The destination port is:

80

The communication is therefore associated with HTTP traffic.

Q9. How many TCP endpoint entries are present?

Answer: The TCP Endpoint view contains 9 endpoint/port entries.

Q10. How many UDP endpoint entries are present?

Answer: The UDP Endpoint view contains 87 endpoint/port entries.

Q11. Does an external IP automatically indicate malicious activity?

Answer: No.

External communication is normal in many network environments. Additional evidence is required before classifying an endpoint or connection as malicious.

Q12. What should be investigated next?

Answer: The prioritized TCP communication should be examined for:

TCP three-way handshake
SYN/ACK/RST/FIN flags
Connection establishment and termination
Retransmissions
Source and destination ports
TCP stream contents
HTTP requests and responses
DNS/domain information
Timing and repeated connections
Potential indicators of compromise

## 10. 📊 Endpoint Summary
Endpoint Type	Count	Investigation Result
Ethernet	87	Lower-layer endpoint activity identified
IPv4	18	Multiple internal and external endpoints identified
IPv6	5	IPv6 and multicast/link-local activity observed
TCP	9	Priority TCP communication identified
UDP	87	Multiple UDP endpoint/port combinations identified

## 11. 🧠 Analyst Assessment

Overall Status: 🟡 Requires Further Investigation

The endpoint analysis successfully established the network hosts and endpoint activity present in the PCAP.

The most relevant communication identified during this investigation is:

10.251.23.139 → 86.66.0.227:80

with:

116 packets
37 KB
TCP
Destination Port: 80

This communication is a suitable target for deeper analysis because it represents significant traffic between an internal endpoint and an external destination.

However, no malicious activity is confirmed at this stage.

Endpoint statistics provide context and prioritization but are insufficient to determine the intent of the communication.

## 12. ✅ Conclusion

Investigation #3 identified the endpoint landscape of the nb6-startup.pcap capture across Ethernet, IPv4, IPv6, TCP and UDP.

The capture contains:

87 Ethernet endpoints
18 IPv4 endpoints
5 IPv6 endpoints
9 TCP endpoint/port entries
87 UDP endpoint/port entries

The primary communication selected for deeper investigation was:

10.251.23.139 → 86.66.0.227:80

with 116 packets and approximately 37 KB of TCP traffic.

This investigation does not confirm malicious activity. Instead, it establishes a clear priority connection for the next stage of NetTrace.

Final Assessment: Endpoint analysis established the network communication baseline and identified a priority TCP/HTTP connection for deeper packet-level investigation.
