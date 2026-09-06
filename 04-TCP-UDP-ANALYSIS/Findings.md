# Investigation 04 — TCP & UDP Analysis Findings

## 1. Investigation Summary

TCP and UDP traffic from `nb6-startup.pcap` was analyzed using Wireshark to
understand transport-layer communication, TCP connection establishment,
TCP data transfer, connection termination, UDP communication, and
DNS-based application traffic.

The investigation was performed using packet-level inspection and Wireshark
display filters. Representative packets were selected to document the
observed transport-layer behavior.

---

# 2. TCP Analysis Findings

## Finding 1 — TCP Traffic Identified

TCP traffic was isolated using the Wireshark display filter:

```text
tcp
```

The capture contains multiple TCP conversations involving internal and
external endpoints.

A representative TCP communication was observed between:

10.251.23.139:35383
        ↕
86.66.0.227:80

Port 80 indicates that the TCP session was associated with HTTP traffic.


## Finding 2 — Complete TCP Three-Way Handshake Observed

A complete TCP connection establishment sequence was identified:

SYN → SYN/ACK → ACK

The observed sequence was:

10.251.23.139:35383 → 86.66.0.227:80
[SYN]

86.66.0.227:80 → 10.251.23.139:35383
[SYN, ACK]

10.251.23.139:35383 → 86.66.0.227:80
[ACK]

This confirms that the TCP connection was successfully established before
application data was exchanged.

Assessment

Normal TCP connection establishment.


## Finding 3 — TCP Header and Control Flags

The TCP header was inspected by expanding the Transmission Control
Protocol section in Wireshark.

Important TCP fields observed include:

Source Port
Destination Port
Sequence Number
Acknowledgment Number
TCP Header Length
TCP Flags
Window Size
Checksum
TCP Options

A representative SYN/ACK packet showed:

Source Port:       80
Destination Port:  35383
Sequence Number:   0
Acknowledgment:    1
Flags:             SYN, ACK

The SYN flag indicates synchronization of sequence numbers during connection
establishment, while ACK confirms acknowledgment of the previous TCP
segment.

Assessment

The TCP header fields and flag combination are consistent with the
second stage of a standard TCP three-way handshake.


## Finding 4 — TCP Application Data Transfer Observed

After the TCP connection was established, packets containing application
payload were observed.

A representative packet showed:

Sequence Number:       1
Acknowledgment Number: 1
TCP Segment Length:    5 bytes
Flags:                 PSH, ACK

The presence of TCP payload demonstrates that application data was
transmitted after connection establishment.

The PSH flag indicates that the receiver should pass the received data
to the application without waiting for additional buffering, while ACK
acknowledges previously received data.

Assessment

TCP application data transfer was successfully observed.


## Finding 5 — HTTP Application Traffic Identified

The TCP session was reconstructed using Follow → TCP Stream.

The reconstructed conversation contained an HTTP request similar to:

GET /... HTTP/1.1
Host: tv.nb6dsl.neufbox.neuf.fr
User-Agent: Wget

The server response contained:

HTTP/1.1 200 OK
Server: Apache
Content-Type: application/xml

The response also contained readable XML application data.

This confirms that the analyzed TCP session carried HTTP application-layer
traffic over TCP port 80.

Assessment

HTTP communication successfully identified and reconstructed.


## 3. TCP Connection Termination Findings
## Finding 6 — TCP FIN Flag Observed

A TCP packet containing the FIN flag was captured and inspected.

The FIN flag is used by TCP to indicate that an endpoint has finished
sending data and wants to initiate an orderly connection termination.

The presence of a FIN packet demonstrates that TCP connection termination
behavior was observed in the capture.

Assessment

TCP graceful termination activity observed.

The FIN flag by itself is not evidence of malicious activity.

Evidence
TCP_flags-fin.png
## Finding 7 — TCP RST/ACK Observed

A separate TCP packet was observed with:

[RST, ACK]

The packet was associated with:

10.251.23.139:35383 → 86.66.0.227:80

The RST flag indicates that the TCP connection was reset rather than
closed through the normal FIN-based termination process.

Assessment

A TCP reset was observed and should be treated as a notable transport-layer
event.

However, an RST packet alone does not prove malicious activity.
Connection resets can occur for legitimate reasons such as application
termination, closed ports, aborted connections, or protocol errors.


## 4. TCP Anomaly Assessment
## Finding 8 — No Significant Retransmission Behavior Identified

The TCP traffic reviewed during the investigation did not show significant
retransmission behavior.

No repeated TCP segment was identified as a clear retransmission in the
reviewed traffic.

The observed RST/ACK packet was documented separately as connection-reset
behavior and was not automatically classified as malicious.

Assessment

No significant TCP retransmission anomaly identified in the reviewed
traffic.

Note: This conclusion applies to the traffic reviewed during this
investigation and does not claim that every possible TCP anomaly was
exhaustively eliminated from the entire capture.

## 5. UDP Analysis Findings
## Finding 9 — UDP Traffic Identified

UDP traffic was isolated using:

udp

The capture contains UDP-based communication including:

DHCP
DNS

DHCP traffic was observed using the standard DHCP client/server ports.

A DHCP Discover packet was observed with:

Source:       0.0.0.0
Destination:  255.255.255.255
Protocol:     DHCP

This represents a broadcast DHCP discovery process used by a client to
locate an available DHCP server.

Assessment

UDP-based DHCP communication observed.

## 6. UDP Header Analysis
## Finding 10 — UDP Header Fields Identified

A representative DNS packet was inspected by expanding the
User Datagram Protocol section in Wireshark.

The following UDP header fields were observed:

Source Port:       39796
Destination Port:  53
Length:             47 bytes
Checksum:           0xdbdb

The destination port 53 identifies DNS service traffic.

Unlike TCP, UDP does not contain:

Sequence numbers
Acknowledgment numbers
SYN flag
ACK flag
FIN flag
RST flag

UDP instead provides a lightweight transport mechanism based primarily on
source/destination ports, length, and checksum.

Assessment

Valid UDP packet structure observed.


## 7. UDP Application Traffic
## Finding 11 — DNS Traffic Identified

DNS traffic was identified as an application-layer protocol operating over
UDP.

The communication used:

Client:  95.136.242.54:39796
Server:  109.0.66.10:53

The DNS packet contained:

Standard query
Transaction ID: 0x0002
Query Type: AAAA
Hostname: stats.neufbox.neuf.fr

A corresponding DNS response was also observed with the matching
transaction ID.

Assessment

Normal DNS query/response communication identified over UDP.


## 8. DNS Query and Response Correlation
## Finding 12 — DNS Request/Response Successfully Correlated

The DNS transaction was correlated using the transaction ID:

Query:
Transaction ID: 0x0002
Query: stats.neufbox.neuf.fr
Type: AAAA

Response:
Transaction ID: 0x0002
Response for stats.neufbox.neuf.fr

The matching transaction ID indicates that the response corresponds to the
observed DNS request.

The AAAA query type requests an IPv6 address associated with the queried
hostname.

Assessment

DNS query and response successfully correlated.

## 9. Investigation Questions & Answers

1	Was TCP traffic identified?	Yes

2	Was a complete TCP three-way handshake observed?	Yes

3	What TCP flags were observed during connection establishment?	SYN and ACK

4	Was TCP application data transferred?	Yes

5	What application protocol was identified over TCP?	HTTP

6	Was the TCP application stream reconstructed?	Yes, using Follow TCP Stream

7	Was a TCP FIN flag observed?	Yes

8	Was a TCP RST/ACK packet observed?	Yes

9	Were significant retransmissions identified in the reviewed traffic?	No

10	Was UDP traffic identified?	Yes

11	What UDP-based protocols were observed?	DHCP and DNS

12	What UDP header fields were examined?	Source Port, Destination Port, Length, Checksum

13	Was DNS traffic observed over UDP?	Yes

14	Was a DNS query/response pair identified?	Yes

15	What DNS query type was observed?	AAAA

## 10. Overall Findings

The TCP/UDP investigation demonstrated both connection-oriented and
connectionless transport-layer communication within the capture.

TCP

The analysis confirmed:

Successful TCP three-way handshake
TCP header and flag activity
Application data transfer
HTTP communication over TCP port 80
TCP stream reconstruction
FIN-based connection termination activity
RST/ACK reset activity
No significant retransmission behavior in the reviewed traffic
UDP

The analysis confirmed:

UDP communication
DHCP broadcast traffic
DNS traffic over UDP port 53
UDP header structure
DNS query/response communication
AAAA DNS query activity

## 11. Security Interpretation

The observed traffic is primarily consistent with normal network
communication.

The TCP RST/ACK packet represents a notable transport-layer event, but it
cannot be considered malicious without additional context.

The DNS and DHCP traffic observed in the capture also represents expected
network functionality.

No significant retransmission behavior was identified during the reviewed
TCP analysis.

## 12. Conclusion

Investigation 04 successfully analyzed TCP and UDP transport-layer behavior
using Wireshark.

The investigation demonstrated the complete TCP communication lifecycle from
connection establishment through application data transfer and observed
termination/reset behavior. HTTP traffic was reconstructed using Follow TCP
Stream.

UDP analysis identified DHCP and DNS traffic, with the UDP header and DNS
query/response relationship examined at packet level.

Overall, the reviewed traffic did not reveal a significant transport-layer
anomaly requiring escalation. The observed TCP RST/ACK was documented as a
notable event but was not classified as malicious without supporting
evidence.
