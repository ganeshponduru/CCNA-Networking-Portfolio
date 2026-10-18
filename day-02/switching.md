# Circuit Switching vs Packet Switching

## Circuit switching
Circuit switching establishes a dedicated end-to-end path or circuit for a communication session. In traditional circuit-switched systems, resources are reserved for the session and released when it ends. Example: traditional telephone networks.

## Packet switching
Packet switching sends data in packets that share network links with other communications. Routers forward packets hop by hop. Packets can experience different delays and may arrive out of order. IP networks and the public Internet use packet switching.

| Feature | Circuit switching | Packet switching |
|---|---|---|
| Resources | Dedicated circuit/resources for the session | Links shared among flows |
| Setup | Circuit establishment typically required | No dedicated end-to-end circuit required by IP |
| Resource use | Reserved resources may be idle during silence | Shared capacity can be used by other traffic |
| Delay | Often more predictable after setup in traditional designs | Varies with congestion and queueing |
| Example | Traditional circuit-switched telephone call | Web browsing over IP |

**Important correction:** Packet switching is not a feature of TCP alone. IP forwards packets; TCP is a transport protocol that provides reliable, ordered byte-stream delivery. UDP also runs over IP but does not provide TCP-style reliability. TCP divides a byte stream into segments as needed; do not assume every packet has the same size or that packets always arrive out of order.

**Interview answer:** Circuit switching reserves a circuit for a session; packet switching sends packets over shared links and forwards them hop by hop.