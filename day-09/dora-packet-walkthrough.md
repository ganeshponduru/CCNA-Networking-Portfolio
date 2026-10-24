# DHCP DORA Packet Walkthrough

In this lab, I used Wireshark to observe the DHCP messages exchanged between a client and a DHCP server. I captured traffic on two links, R1–Switch1 and R2–Switch1, and compared the packets I could see.

## DHCP basics

DHCP allows a client to obtain network settings such as an IPv4 address, subnet mask, default gateway, and DNS server.

- DHCP client uses UDP port 68.
- DHCP server uses UDP port 67.

## DORA sequence

1. **Discover:** The client looks for a DHCP server. In my captures, the Discover packet uses source IP `0.0.0.0`, destination IP `255.255.255.255`, and UDP ports 68 → 67.
2. **Offer:** The server replies with an address it is offering to the client.
3. **Request:** The client requests the offered address and identifies the DHCP server it selected.
4. **ACK:** The server acknowledges the request and sends the lease configuration.

## What I observed on R1–Switch1

The DHCP packets I examined were frames 8, 10, 11, and 12. They share transaction ID `0xa5338232`.

| Frame | Message | IPv4 source | IPv4 destination | UDP ports |
|---:|---|---|---|---|
| 8 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 10 | Offer | `192.168.1.100` | `192.168.1.1` | 67 → 68 |
| 11 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 12 | ACK | `192.168.1.100` | `192.168.1.1` | 67 → 68 |

In the Discover details, I saw client hardware address `00:50:79:66:68:00` and BOOTP flags `0x0000`, meaning the broadcast flag was not set. The Offer shows `192.168.1.1` as the offered client address. The ACK includes options for the subnet mask, router, DNS servers, and lease timers.

## What I observed on R2–Switch1

In the second capture, the packet list showed this sequence:

| Frame | Message | IPv4 source | IPv4 destination | UDP ports |
|---:|---|---|---|---|
| 7 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 9 | Offer | `192.168.1.100` | `255.255.255.255` | 67 → 68 |
| 10 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 11 | ACK | `192.168.1.100` | `255.255.255.255` | 67 → 68 |

I noticed that the Offer and ACK destinations were different in the two captures. On R1–Switch1, the destination shown for those messages was `192.168.1.1`; on R2–Switch1, it was `255.255.255.255`.

The R2 screenshot shows the packet list, but I did not capture the expanded BOOTP flags or Ethernet destination details for those packets. So, from the screenshots I have, I can describe the difference I observed, but I cannot say for certain why the R2 Offer and ACK were broadcast.

The console in the R2 screenshot also shows R2 receiving address `192.168.1.3/24` on `GigabitEthernet1/0`.

## What I learned

By comparing the captures, I learned that DHCP uses the same DORA sequence, but the IPv4 destination shown for the Offer and ACK was not the same at both capture points. To explain the exact reason, I would need to inspect more packet details rather than guess from the packet list alone.

## DHCP options and header fields

For the DHCP options and BOOTP header fields, see [DHCP options and header fields](dhcp-options-and-fields.md). For the packet-by-packet capture notes, see [R1–Switch1](capture-r1-switch.md) and [R2–Switch1](capture-r2-switch.md).
