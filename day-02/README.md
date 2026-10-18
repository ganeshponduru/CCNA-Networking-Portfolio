# Day 02 — Switching, Traffic Types, and IPv4 Basics

In these notes, I organized what I learned about how networks move data, how switches handle Ethernet traffic, and how IPv4 addresses are represented.

## Topics I covered

- [Circuit switching vs packet switching](switching.md)
- [Unicast, broadcast, multicast, and Ethernet forwarding](traffic-types.md)
- [Collision domains and broadcast domains](domains.md)
- [Common network topologies](topologies.md)
- [IPv4 addresses, address classes, and private ranges](ipv4-addressing.md)
- [Binary and decimal conversion](binary-conversion.md)
- [Questions I can use to revise](interview-questions.md)

## Key takeaways

- Packet switching lets multiple conversations share network links; IP forwards packets, while TCP can provide reliable, ordered delivery.
- A switch learns source MAC addresses and uses its MAC address table to forward Ethernet frames.
- Broadcast traffic is limited to its Layer 2 broadcast domain unless a device or protocol deliberately forwards or relays it.
- A subnet mask helps identify the network portion of an IPv4 address.
- Binary conversion is useful for understanding IPv4 octets and subnet masks.
- Class A/B/C addressing is useful historical background, but modern networks are planned with CIDR prefixes.

## Practice status

These files document my current understanding of the concepts. I have not marked a Packet Tracer or physical-network lab as completed. I'll add configurations, screenshots, and results after I perform and verify the practical work.
