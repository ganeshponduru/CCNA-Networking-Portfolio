# OSI and TCP/IP Reference Models

Reference models help explain network communication by separating functions. OSI is a seven-layer conceptual model; the Internet protocol suite is commonly presented as a four-layer TCP/IP model. Real implementations do not always align perfectly with textbook boundaries.

## OSI layers

| Layer | Responsibility | Examples / concepts | Typical data unit |
|---|---|---|---|
| 7 — Application | Network services used by applications | HTTP, DNS, SMTP, SSH | Data |
| 6 — Presentation | Data representation, encoding, compression, and certain security transformations in the OSI model | Character encoding, data formats; encryption can occur at different layers in real systems | Data |
| 5 — Session | Establishes, manages, and terminates logical sessions in the OSI model | Session/dialog management | Data |
| 4 — Transport | End-to-end process communication; segmentation, ports, reliability/flow control where provided | TCP, UDP | TCP segment / UDP datagram |
| 3 — Network | Logical addressing and routing between networks | IPv4, IPv6, routing | Packet |
| 2 — Data Link | Local-link framing, MAC addressing, link-level error detection | Ethernet, Wi-Fi MAC, VLAN tagging | Frame |
| 1 — Physical | Transmission of signals over a medium | Copper, fiber, radio, connectors, signaling | Bits |

Session and presentation functions are often handled by applications or libraries rather than distinct, universally implemented layers. TLS is commonly described as operating above TCP and below application protocols; do not assume encryption belongs only to OSI Layer 6.

## TCP/IP model

| TCP/IP layer | Approximate OSI mapping | Examples |
|---|---|---|
| Application | OSI 5–7 | HTTP, DNS, SMTP, SSH, DHCP |
| Transport | OSI 4 | TCP, UDP |
| Internet | OSI 3 | IPv4, IPv6, ICMP |
| Network Access / Link | OSI 1–2 | Ethernet, Wi-Fi, physical media |

Some teaching materials use a five-layer model by separating physical and data-link functions. State which model you are using.

## Encapsulation

A simplified path is: application data → TCP segment or UDP datagram → IP packet → link-layer frame → bits/signals. The receiver processes the wrappers in reverse order.

## Hardware and troubleshooting

A NIC provides link-layer and physical connectivity and may offload some processing. It does not implement the entire TCP/IP stack by itself; operating-system networking software and hardware share the work.

When troubleshooting, identify the symptom and scope, then check link/VLAN, IP/subnet/gateway, routing, transport reachability, DNS, and application behavior. Validate hypotheses with evidence.

Switches commonly forward Ethernet frames using MAC addresses; routers forward IP packets between networks. IP does not itself guarantee delivery. IPv4 protocol numbers 6 (TCP) and 17 (UDP) are not port numbers.