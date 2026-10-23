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

The screenshots in this section are from the **R1–Switch1 link**. The packet list and expanded packet details show a DHCP exchange with transaction ID `0xa5338232`.

| Packet | DHCP message | IPv4 source | IPv4 destination | UDP ports |
|---|---|---|---|---|
| 8 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 10 | Offer | `192.168.1.100` | `192.168.1.1` | 67 → 68 |
| 11 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 12 | ACK | `192.168.1.100` | `192.168.1.1` | 67 → 68 |

The Offer and ACK details show `192.168.1.1` as the offered/assigned client address. I will verify this against the full lab addressing and configuration before treating it as the final topology description.

## Uploaded R1–Switch1 screenshots

The screenshots below are the image files uploaded to this directory. Captions identify the evidence set rather than asserting details that are not legible in the file name.

![R1–Switch1 Wireshark evidence 1](./Screenshot%202026-10-23%20225043.png)

![R1–Switch1 Wireshark evidence 2](./Screenshot%202026-10-23%20225205.png)

![R1–Switch1 Wireshark evidence 3](./Screenshot%202026-10-23%20225504.png)

![R1–Switch1 Wireshark evidence 4](./Screenshot%202026-10-23%20225640.png)

![R1–Switch1 Wireshark evidence 5](./Screenshot%202026-10-23%20225849.png)

![R1–Switch1 Wireshark evidence 6](./Screenshot%202026-10-23%20231757.png)

![R1–Switch1 Wireshark evidence 7](./Screenshot%202026-10-23%20232506.png)

## Evidence still to add

- Additional screenshots from the other capture point (R2–Switch)
- DHCP configuration and client/server verification output
- A final comparison of BOOTP flags and IP/Ethernet destinations in both captures

## Scope and accuracy

I will distinguish what I configured in the lab from what is visible in each capture. A packet capture only proves what was observed at that capture point; it does not by itself prove every device's complete configuration.

## Notes

- [DORA packet walkthrough](dora-packet-walkthrough.md)
- [DHCP options and header fields](dhcp-options-and-fields.md)
- [Configuration and verification checklist](configuration-and-verification.md)
- [Capture analysis: R1–Switch1](capture-r1-switch.md)
