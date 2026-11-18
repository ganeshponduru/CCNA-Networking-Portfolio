# Traceroute Packet Analysis

## Traceroute and Tracert

Both commands help identify the path packets take to a destination and show the round-trip time (RTT) for each responding hop.

| Utility | Common platform | Probe type |
|---|---|---|
| traceroute | Linux / Unix | UDP by default on common implementations |
| tracert | Windows | ICMP Echo Request |

The probe method can vary between traceroute implementations and options. The table describes the default behavior covered here. The PDF and session describe tracing up to 30 hops; the actual maximum can depend on the utility and its settings.

## UDP probes in traceroute

The Linux/Unix UDP traceroute method sends three probes for each TTL value by default. It starts with TTL 1 and increases the TTL for the next group of probes.

The PDF describes the UDP destination-port range as **33434–33523**. The destination port is varied across probes; the source port is normally selected from an ephemeral range.

The three probes are not three different kinds of packets. They are separate attempts for the same hop, which give multiple RTT samples and help show whether responses are consistent.

## ICMP responses

Two ICMP responses are important when reading a UDP traceroute capture.

| ICMP message | Type | Code | Meaning |
|---|---:|---:|---|
| Time Exceeded | 11 | 0 | A router discarded the probe because its TTL expired in transit |
| Destination Unreachable — Port Unreachable | 3 | 3 | The destination was reached, but the UDP destination port was closed or unavailable |

When the destination returns Type 3, Code 3, the UDP traceroute process treats that as the end of the trace. It does not mean that every application service on the destination is working. It only tells us that the destination IP responded with a port-unreachable message for the UDP probe.

## Packet flow example

For the R5-to-destination example:

1. R5 sends three UDP probes with TTL 1.
2. R4 receives them, expires the TTL, and returns ICMP Type 11, Code 0.
3. R5 sends the next three probes with TTL 2. R2 returns the Time Exceeded messages.
4. R5 repeats with TTL 3. R1 returns the Time Exceeded messages.
5. R5 sends probes with TTL 4. The destination is reached. If the UDP destination port is closed, it returns ICMP Type 3, Code 3.

The ICMP response is sent back to the source. The source uses the response to identify the router that expired the TTL.

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
