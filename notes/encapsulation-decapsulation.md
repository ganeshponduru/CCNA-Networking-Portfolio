# Encapsulation and Decapsulation

## Encapsulation

Encapsulation is the process in which protocol layers add their own control information as application data is prepared for transmission.

1. The application creates data, such as an HTTP request.
2. TCP adds source/destination ports and transport-control fields.
3. IPv4 adds source/destination IP addresses and a protocol field.
4. Ethernet adds a frame header with source and next-hop destination MAC addresses, plus a trailer/FCS.
5. The interface transmits signals/bits over the medium.

This is a simplified conceptual sequence; actual structure depends on the protocols in use.

## Decapsulation

At the receiving host, the link layer processes the frame, IP processes the packet, and the transport layer delivers data to the appropriate socket/application. Each layer interprets information relevant to its function.

## Remote website example

For traffic to a remote server, the destination IP identifies the remote host. The Ethernet destination MAC on the local network is normally the MAC address of the next hop, often the default gateway—not the remote server's MAC. Routers use a new link-layer frame for each routed link; IP forwarding may also update fields such as TTL.

## Header summary

| Protocol | Example information |
|---|---|
| Application | Application message/request |
| TCP | Source port, destination port, sequence and acknowledgment numbers, flags |
| IPv4 | Source IP, destination IP, TTL, protocol |
| Ethernet | Source MAC, next-hop destination MAC, EtherType, FCS/trailer |

See the original [encapsulation diagram](../diagrams/encapsulation-flow.svg).

## Interview points

- Encapsulation: adding protocol-specific headers and sometimes trailers as data moves down the stack.
- Decapsulation: interpreting/removing the relevant wrappers as data moves up the receiving stack.
- Ethernet headers change at each routed link because each link has its own local delivery context.
- For remote destinations, the local Ethernet destination is usually the next-hop router's MAC.