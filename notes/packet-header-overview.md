# Packet-Header Overview

Packet analyzers such as Wireshark display captured frames and decode protocol fields. They show captured data and decoded fields—not a direct view of every physical signal or bit transmitted on the medium.

## What to inspect

| Layer / header | Useful fields | What they help answer |
|---|---|---|
| Frame/capture metadata | Timestamp, captured/original length, interface details when available | When/where was it captured? Was it truncated? |
| Ethernet II | Source MAC, destination MAC, EtherType, VLAN tag if present | Which link-layer devices are involved on this segment? |
| IPv4 | Source/destination IP, TTL, protocol, fragmentation fields | Which IP endpoints are represented? Which transport protocol is carried? |
| TCP | Source/destination ports, flags, sequence/acknowledgment numbers, window | Is a handshake occurring? Are retransmissions or resets visible? |
| UDP | Source/destination ports, length, checksum | Which transport endpoints and datagram length are shown? |
| Application protocol | Decoded fields, where supported and visible | What application-level information is available? Is payload encrypted? |

## Example interpretation

If a captured frame has an Ethernet destination MAC matching the local gateway, an IPv4 destination address belonging to a remote server, and TCP destination port 443, it may represent traffic being sent toward a remote HTTPS endpoint through the next-hop router. Port 443 alone does not prove the packet is HTTPS; protocol context and capture conditions matter. HTTP/3 commonly uses QUIC over UDP 443.

## Limitations

- On a routed path, Ethernet headers change for each link; IP source/destination usually remain end-to-end except for changes such as NAT. TTL is decremented by routers.
- Capture location, offloading, encryption, truncation, and filters affect visibility.
- A capture shows traffic visible at its capture point; it does not guarantee every packet was captured.
- One TCP packet alone may not prove a complete connection or root cause.

Use the [packet-analysis exercise](../packet-analysis/README.md) to record your own capture and findings.