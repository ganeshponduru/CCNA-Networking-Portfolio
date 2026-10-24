# Wireshark Capture Notes — R2 to Switch1

## Capture point

The supplied screenshot shows Wireshark capturing on **R2 GigabitEthernet1/0 to Switch1 Ethernet3**.

## Observed DHCP exchange

The packet list shows this sequence:

| Frame | Message | IPv4 source | IPv4 destination | UDP ports |
|---:|---|---|---|---|
| 7 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 9 | Offer | `192.168.1.100` | `255.255.255.255` | 67 → 68 |
| 10 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 11 | ACK | `192.168.1.100` | `255.255.255.255` | 67 → 68 |

The screenshot shows DHCP Offer and ACK packets using the limited IPv4 broadcast destination. The packet list alone does not show the BOOTP broadcast flag, offered address (`yiaddr`), client hardware address (`chaddr`), or Ethernet destination MAC in enough detail to explain why this server/client exchange used broadcast replies.

## R2 interface evidence

The adjacent R2 console shows a DHCP address assignment message for `GigabitEthernet1/0`: address `192.168.1.3`, mask `255.255.255.0`, hostname R2. This is evidence about R2's own interface DHCP client configuration; it should not be confused with the DHCP server address `192.168.1.100` shown as the source of the captured Offer and ACK.

## Comparison with R1–Switch1 capture

The earlier R1–Switch1 capture showed Discover and Request to `255.255.255.255`, but Offer and ACK to `192.168.1.1`. In this R2–Switch1 capture, all four DHCP messages shown use `255.255.255.255` as the IPv4 destination for Discover/Request and Offer/ACK.

Possible factors include the client's BOOTP broadcast flag, DHCP client/server behavior, and the exact network arrangement. Do not state a definite cause until the detailed fields are checked.

## Fields to inspect next

Open the R2 capture's Discover, Offer, Request, and ACK packets and compare:

1. BOOTP flags, especially whether the broadcast flag (`0x8000`) is set
2. IPv4 destination and Ethernet destination MAC address
3. `ciaddr` (client IP address) and `yiaddr` (offered client IP address)
4. `chaddr` (client hardware address)
5. Transaction ID and server identifier (DHCP option 54)

A packet addressed to IPv4 `255.255.255.255` is normally carried in an Ethernet frame addressed to `ff:ff:ff:ff:ff:ff` on the local Ethernet segment, but verify the actual frame fields in Wireshark.

## Evidence limitation

This note is based on the screenshot supplied in the conversation. The screenshot itself has not yet been uploaded as a file in the GitHub repository, so this page does not embed it.
