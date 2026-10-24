# R2–Switch1 DHCP Capture

## Capture link

The screenshot shows Wireshark capturing on **R2 GigabitEthernet1/0 to Switch1 Ethernet3**.

## DHCP packets

| Frame | Message | IPv4 source | IPv4 destination | UDP ports |
|---:|---|---|---|---|
| 7 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 9 | Offer | `192.168.1.100` | `255.255.255.255` | 67 → 68 |
| 10 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 11 | ACK | `192.168.1.100` | `255.255.255.255` | 67 → 68 |

The Offer and ACK use the limited IPv4 broadcast destination in the packet list.

## R2 console

The console shows R2 receiving DHCP address `192.168.1.3`, mask `255.255.255.0`, on `GigabitEthernet1/0`. This is R2's own interface address and is separate from the DHCP server address `192.168.1.100`.

## Compare with R1–Switch1

The R1–Switch1 capture shows Offer and ACK addressed to `192.168.1.1`; this R2–Switch1 capture shows Offer and ACK addressed to `255.255.255.255`.

The screenshot does not show the expanded BOOTP flags or Ethernet destination MAC addresses, so it is not enough to identify the exact reason for the broadcast replies. Those fields can be checked if a detailed packet view is captured later.
