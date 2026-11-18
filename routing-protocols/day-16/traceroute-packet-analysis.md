# Traceroute Packet Analysis

## Traceroute and Tracert

Both commands help identify the path packets take to a destination and show the round-trip time (RTT) for each responding hop.

| Utility | Common platform | Probe type |
|---|---|---|
| traceroute | Linux / Unix | UDP by default on common implementations |
| tracert | Windows | ICMP Echo Request |

The probe method can vary between traceroute implementations and options. The practical capture in this lab shows UDP probes.

## UDP probes in the capture

The capture shows three UDP probes for each TTL value. The destination IP address is **4.4.4.4**, and the source IP address is **192.168.12.1**.

| Probe group | TTL | UDP source port | UDP destination ports |
|---|---:|---:|---|
| First | 1 | 49212 | 33434, 33435, 33436 |
| Second | 2 | 49215 | 33437, 33438, 33439 |
| Third | 3 | 49218 | 33440, 33441, 33442 |

The capture shows the source port increasing between probe groups and the destination port increasing for each probe.

## ICMP responses in the capture

Two ICMP responses are important when reading a UDP traceroute capture.

| ICMP message | Type | Code | Meaning |
|---|---:|---:|---|
| Time Exceeded | 11 | 0 | A router discarded the probe because its TTL expired in transit |
| Destination Unreachable — Port Unreachable | 3 | 3 | The destination side returned a port-unreachable response for the UDP probe |

The observed response sequence was:

1. **TTL 1:** R2 returned ICMP Type 11, Code 0. Its response source address was 192.168.12.2.
2. **TTL 2:** R3 returned ICMP Type 11, Code 0. Its response source address was 192.168.23.3.
3. **TTL 3:** R4 returned ICMP Type 3, Code 3. Its response source address was 192.168.34.4, while the original probe destination was 4.4.4.4.

The Type 3, Code 3 response is the stopping response in this UDP traceroute test. It does not prove that every application service on the destination is working. It tells us that a Port Unreachable response was returned for the UDP probe.

## What to inspect in Wireshark

For each probe and response, check:

- The source and destination IP addresses in the IP header.
- The TTL value in each outgoing UDP probe.
- The UDP source and destination ports.
- The ICMP Type and Code in the response.
- The source IP address of the router sending the ICMP response.
- The time between the probe and its response.

A useful display filter for an initial review is:

~~~text
udp || icmp
~~~

Then inspect individual packets rather than relying only on the packet-list summary.

## Round Trip Time (RTT)

RTT is the time between sending a probe and receiving its response. Traceroute displays an RTT measurement for each probe that receives a response.

The three RTT values shown for one hop are separate measurements. They can differ because of congestion, queueing, processing delay, and other network conditions. RTT is not the same as one-way latency.
