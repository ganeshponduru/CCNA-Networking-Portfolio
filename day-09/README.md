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

## R2–Switch1 comparison received

A second capture screenshot was supplied showing Wireshark on **R2 GigabitEthernet1/0 to Switch1 Ethernet3**. Its packet list shows:

| Packet | DHCP message | IPv4 source | IPv4 destination | UDP ports |
|---|---|---|---|---|
| 7 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 9 | Offer | `192.168.1.100` | `255.255.255.255` | 67 → 68 |
| 10 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 11 | ACK | `192.168.1.100` | `255.255.255.255` | 67 → 68 |

Unlike the R1–Switch1 capture, the Offer and ACK in this packet list use the limited broadcast IPv4 destination. This establishes the observed difference, but not its cause. The BOOTP flags and Ethernet destination MAC addresses must be inspected in the individual R2 packets before explaining why the replies were broadcast.

The screenshot also shows R2 reporting DHCP address `192.168.1.3/24` on `GigabitEthernet1/0`. This is R2's own interface address assignment and is not the DHCP server address `192.168.1.100`.

- [Capture analysis: R2–Switch1](capture-r2-switch.md)

## Evidence still to add

- Detailed packet-field screenshots for the R2 Offer and ACK, including BOOTP flags and Ethernet destination MAC
- DHCP configuration and client/server verification output
- Confirmation of the intended gateway/client addressing, because the R1 capture shows `192.168.1.1` as the offered client address

## Scope and accuracy

I will distinguish what I configured in the lab from what is visible in each capture. A packet capture only proves what was observed at that capture point; it does not by itself prove every device's complete configuration.

## Notes

- [DORA packet walkthrough](dora-packet-walkthrough.md)
- [DHCP options and header fields](dhcp-options-and-fields.md)
- [Configuration and verification checklist](configuration-and-verification.md)
- [Capture analysis: R1–Switch1](capture-r1-switch.md)
- [Capture analysis: R2–Switch1](capture-r2-switch.md)
