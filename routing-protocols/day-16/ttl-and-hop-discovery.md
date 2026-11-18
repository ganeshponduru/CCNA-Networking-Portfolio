# TTL and Hop Discovery

## What is TTL?

TTL stands for **Time to Live**. It is an 8-bit field in the IPv4 header, so its value can range from 0 to 255. The sender sets an initial value. Every router that forwards the packet reduces the TTL by at least one.

TTL is important because it stops an IP packet from circulating forever if a routing loop occurs. When the TTL expires, the router drops the packet instead of forwarding it and normally sends an ICMP Time Exceeded message back to the source.

For IPv4, the ICMP message used for TTL expiry is:

| Field | Value |
|---|---|
| ICMP Type | 11 — Time Exceeded |
| ICMP Code | 0 — TTL exceeded in transit |

**Type 3, Code 3 is different:** it means Destination Unreachable — Port Unreachable.

## How traceroute uses TTL

Traceroute sends probes with a low TTL first and increases the TTL for each new round. Each router along the path reduces the TTL. When it reaches zero, that router drops the probe and normally sends an ICMP Time Exceeded message.

In my packet capture, the test was run from R1 toward **4.4.4.4**. The capture was taken on the R1–R2 link.

| Probe TTL | Router response | Response address | ICMP result |
|---:|---|---|---|
| 1 | R2 | 192.168.12.2 | Type 11, Code 0 |
| 2 | R3 | 192.168.23.3 | Type 11, Code 0 |
| 3 | R4 | 192.168.34.4 | Type 3, Code 3 |

The first two TTL values expire at intermediate routers. With TTL 3, the probe reaches the destination address 4.4.4.4, and the final device returns a Port Unreachable message. The response source address visible in the capture is 192.168.34.4.

## Why the TTL value increases

With TTL 1, the first router cannot forward the probe beyond itself. With TTL 2, the first router forwards it and the second router expires it. Each new round allows the probe to travel one router farther.

This is how traceroute discovers the path without needing login access to every intermediate router.

## TTL and routing loops

Suppose R1 forwards a packet to R2, R2 forwards it to R3, and R3 sends it back toward R1 because of incorrect routes. The packet can keep circulating and consuming network resources.

TTL limits the number of router forwarding steps. Once it expires, the packet is discarded and an ICMP Time Exceeded message is normally generated. This prevents that packet from looping indefinitely.

## Points to remember

- TTL is in the IP header, not the TCP or UDP header.
- A router reduces TTL when forwarding an IPv4 packet.
- TTL expiry in transit is ICMP Type 11, Code 0.
- Traceroute uses controlled TTL values to reveal each hop.
- The TTL field has a maximum representable value of 255, but traceroute starts its probes at 1; it does not begin with 255.
