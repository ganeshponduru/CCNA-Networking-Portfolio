# Day 9 — DHCP and DORA

I completed a DHCP lab and captured the DHCP exchange in Wireshark from two links: R1–Switch1 and R2–Switch1.

## What I covered

- DHCP client and server roles
- DORA: Discover, Offer, Request, Acknowledgment (ACK)
- DHCP over UDP ports 67 and 68
- DHCP/BOOTP header fields and options
- DHCP configuration and verification commands
- Comparing DHCP packets in Wireshark

## R1–Switch1 capture

The packet details show transaction ID `0xa5338232`.

| Packet | DHCP message | IPv4 source | IPv4 destination | UDP ports |
|---|---|---|---|---|
| 8 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 10 | Offer | `192.168.1.100` | `192.168.1.1` | 67 → 68 |
| 11 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 12 | ACK | `192.168.1.100` | `192.168.1.1` | 67 → 68 |

The Offer shows `192.168.1.1` as the offered client address. This should be compared with the gateway address used in the lab.

### R1–Switch1 screenshots

![R1–Switch1 DHCP capture 1](./Screenshot%202026-10-23%20225043.png)

![R1–Switch1 DHCP capture 2](./Screenshot%202026-10-23%20225205.png)

![R1–Switch1 DHCP capture 3](./Screenshot%202026-10-23%20225504.png)

![R1–Switch1 DHCP capture 4](./Screenshot%202026-10-23%20225640.png)

![R1–Switch1 DHCP capture 5](./Screenshot%202026-10-23%20225849.png)

![R1–Switch1 DHCP capture 6](./Screenshot%202026-10-23%20231757.png)

![R1–Switch1 DHCP capture 7](./Screenshot%202026-10-23%20232506.png)

## R2–Switch1 capture

The packet list shows broadcast IPv4 destinations for all four DHCP messages.

| Packet | DHCP message | IPv4 source | IPv4 destination | UDP ports |
|---|---|---|---|---|
| 7 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 9 | Offer | `192.168.1.100` | `255.255.255.255` | 67 → 68 |
| 10 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 11 | ACK | `192.168.1.100` | `255.255.255.255` | 67 → 68 |

The console in the screenshot shows R2 receiving `192.168.1.3/24` on `GigabitEthernet1/0`.

![R2–Switch1 DHCP capture and R2 console](./Screenshot%202026-10-23%20233448.png)

The R2 screenshot does not show the expanded BOOTP flags or Ethernet destination fields, so the exact reason for its broadcast Offer and ACK is not confirmed.

## Files

- [DORA packet walkthrough](dora-packet-walkthrough.md)
- [DHCP options and header fields](dhcp-options-and-fields.md)
- [DHCP configuration and verification commands](configuration-and-verification.md)
- [R1–Switch1 packet details](capture-r1-switch.md)
- [R2–Switch1 packet details](capture-r2-switch.md)
