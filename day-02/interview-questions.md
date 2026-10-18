# Day 2 — Interview and Revision Questions

1. **Circuit switching vs packet switching?** Circuit switching establishes a dedicated circuit for a session; packet switching sends packets over shared links.
2. **Is packet switching a TCP feature?** No. IP networks are packet-switched; TCP is a transport protocol that provides reliable, ordered delivery.
3. **What are common protocol data units?** Bits at the physical layer, frames at the data-link layer, packets at the network layer, and TCP segments / UDP datagrams at the transport layer.
4. **Define unicast, broadcast, multicast.** One-to-one, one-to-all within the relevant broadcast domain, and one-to-a-group.
5. **What is the Ethernet broadcast MAC?** FF:FF:FF:FF:FF:FF.
6. **Does a switch always send every frame out every port?** No. It forwards known unicast toward the destination port; broadcast and unknown unicast are generally flooded within the VLAN.
7. **What is a collision domain?** A shared half-duplex Ethernet segment where transmissions can collide.
8. **Hub vs switch?** A hub repeats signals at Layer 1; a switch forwards frames based on Layer 2 MAC addresses.
9. **What is CSMA/CD?** A legacy shared/half-duplex Ethernet method to detect collisions and retry after a backoff.
10. **What is a broadcast domain?** The Layer 2 scope where broadcast frames are flooded, typically one VLAN.
11. **How many bits are in IPv4?** 32 bits, represented as four 8-bit octets.
12. **Why does an IPv4 octet range from 0 to 255?** Eight bits have 256 combinations, numbered 0–255.
13. **What are the RFC 1918 private ranges?** 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16.
14. **Are classful ranges used for modern subnet design?** Modern networks use CIDR prefixes; class A/B/C ranges are historical concepts.
15. **What does /24 mean?** The first 24 bits are the network prefix; eight bits remain for host identifiers.
16. **What is loopback?** 127.0.0.0/8, commonly tested using 127.0.0.1.
17. **What does NAT do?** It translates IP addressing information, commonly allowing private-addressed hosts to share public IPv4 connectivity.
18. **What does ipconfig /all show?** Local network configuration such as IP address, subnet mask, gateway, and DNS servers.

Try answering without reading the notes. If you cannot explain a concept with a simple example, review it before an interview.