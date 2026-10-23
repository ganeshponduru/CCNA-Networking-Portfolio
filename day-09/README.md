# Day 9 — DHCP and DORA Packet Analysis

## Goal

I am documenting how a DHCP client obtains IPv4 configuration and how the exchange appears in Wireshark. I completed a DHCP lab and am adding packet evidence in stages as I review the captures.

## Topics

- DHCP client and server roles
- DORA: Discover, Offer, Request, Acknowledgment (ACK)
- DHCP over UDP ports 67 and 68
- DHCP/BOOTP header fields and options
- DHCP configuration and verification
- Comparing packet destinations across capture points

## Lab evidence received so far

The current Wireshark screenshots are from a capture taken on the **R1–Switch1 link**. They show a DHCP exchange with transaction ID `0xa5338232`:

| Packet | DHCP message | IPv4 source | IPv4 destination | UDP ports |
|---|---|---|---|---|
| 8 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 10 | Offer | `192.168.1.100` | `192.168.1.1` | 67 → 68 |
| 11 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 12 | ACK | `192.168.1.100` | `192.168.1.1` | 67 → 68 |

The Offer and ACK details show the offered/assigned client address as `192.168.1.1`. I will verify this against the full lab addressing and configuration before treating it as the final topology description.

## Evidence still to add

- Additional screenshots from the other capture point (R2–Switch)
- DHCP configuration and client/server verification output
- Any further packet details needed to explain the difference between unicast and broadcast replies

## Scope and accuracy

I will distinguish what I configured in the lab from what is visible in each capture. A packet capture only proves what was observed at that capture point; it does not by itself prove every device's complete configuration.

## Files

- [DORA packet walkthrough](dora-packet-walkthrough.md)
- [DHCP options and header fields](dhcp-options-and-fields.md)
- [Configuration and verification checklist](configuration-and-verification.md)
- [Capture comparison: R1–Switch1](capture-r1-switch.md)
