# Circuit Switching vs Packet Switching

## How I understand circuit switching

Circuit switching establishes a dedicated path or circuit for a communication session. In traditional circuit-switched systems, resources are reserved for that session and released when it ends. A traditional telephone call is a familiar example.

## How I understand packet switching

Packet switching breaks data into packets that share network links with other traffic. Routers forward packets hop by hop. Delay can vary with congestion and queueing, and packets may take different paths or arrive out of order.

The Internet uses packet switching. This is not a feature of TCP alone: IP forwards packets, TCP can provide reliable and ordered byte-stream delivery, and UDP carries datagrams without TCP-style delivery or ordering guarantees.

| Feature | Circuit switching | Packet switching |
|---|---|---|
| Resources | Resources are reserved for the session | Network links are shared by multiple flows |
| Setup | A circuit is typically established first | IP does not require a dedicated end-to-end circuit |
| Resource use | Reserved capacity can be idle during silence | Shared capacity can carry other traffic |
| Delay | Often more predictable after setup in traditional systems | Can vary with congestion and queueing |
| Example | Traditional circuit-switched telephone call | Web traffic over IP |

## What I need to remember

- Circuit switching: a session uses a dedicated circuit or reserved resources.
- Packet switching: data is sent in packets across shared links.
- TCP and UDP are transport-layer protocols; IP is responsible for addressing and forwarding packets between networks.
- Packet size and packet arrival order are not fixed guarantees of packet switching.

## Quick interview answer

Circuit switching reserves a circuit for a communication session. Packet switching sends packets over shared links and forwards them hop by hop.
