# TTL and Hop Discovery

## What is TTL?

TTL stands for **Time to Live**. It is an 8-bit field in the IPv4 header, so its value can range from 0 to 255. The sender sets an initial value. Every router that forwards the packet reduces the TTL by at least one.

TTL is important because it stops an IP packet from circulating forever if a routing loop occurs. When the TTL expires, the router drops the packet instead of forwarding it and normally sends an ICMP Time Exceeded message back to the source.

For IPv4, the ICMP message used for TTL expiry is:

| Field | Value |
|---|---|
| ICMP Type | 11 — Time Exceeded |
| ICMP Code | 0 — TTL exceeded in transit |

**Important:** The correct code for TTL expiry in transit is **Type 11, Code 0**. Type 3, Code 3 is a different message: Destination Unreachable — Port Unreachable.

## How traceroute uses TTL

Traceroute deliberately starts with a small TTL and increases it one step at a time. This makes each router along the path return an ICMP message when the probe's TTL expires.

The example below follows the four-router path discussed in the notes:

- R5 is the source.
- R4 is the first hop.
- R2 is the second hop.
- R1 is the third hop.
- The destination is reached after R1.

| Probe round | Initial TTL | Where it expires | Response seen by R5 |
|---|---:|---|---|
| 1 | 1 | R4 | ICMP Type 11, Code 0 |
| 2 | 2 | R2 | ICMP Type 11, Code 0 |
| 3 | 3 | R1 | ICMP Type 11, Code 0 |
| 4 | 4 | Destination | For UDP traceroute, often ICMP Type 3, Code 3 if the destination UDP port is closed |

The source reads the source address of each ICMP response and displays it as a hop. In the example from the session, the hop addresses were 192.168.45.4, 192.168.24.2, and 192.168.12.1, followed by the destination-side address 192.168.18.8.

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
