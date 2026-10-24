# R1–Switch1 DHCP Capture

## Capture link

Wireshark was capturing on the link labelled **R1 GigabitEthernet1/0 to Switch1 Ethernet2**.

## DHCP packets

Transaction ID: `0xa5338232`

| Frame | Message | IPv4 source | IPv4 destination | UDP ports |
|---:|---|---|---|---|
| 8 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 10 | Offer | `192.168.1.100` | `192.168.1.1` | 67 → 68 |
| 11 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 12 | ACK | `192.168.1.100` | `192.168.1.1` | 67 → 68 |

Client hardware address: `00:50:79:66:68:00`.

### Discover — frame 8

- DHCP message type (option 53): Discover
- BOOTP flags: `0x0000`; the broadcast flag is not set
- Client hardware address: `00:50:79:66:68:00`
- Transaction ID: `0xa5338232`

### Offer — frame 10

- DHCP server source address: `192.168.1.100`
- IPv4 destination: `192.168.1.1`
- UDP ports: 67 → 68
- Offered client address (`yiaddr`): `192.168.1.1`

### Request — frame 11

- IPv4 source/destination: `0.0.0.0` → `255.255.255.255`
- UDP ports: 68 → 67
- Server identifier (option 54): `192.168.1.100`
- Requested IP address (option 50): `192.168.1.1`

### ACK — frame 12

- IPv4 destination: `192.168.1.1`
- UDP ports: 67 → 68
- Server identifier (option 54): `192.168.1.100`
- Subnet mask (option 1): `255.255.255.0`
- Router (option 3), DNS servers (option 6), lease time (option 51), renewal time (option 58), and rebinding time (option 59) appear in the decoded options.

## Comparing with R2–Switch1

In this capture, Offer and ACK use `192.168.1.1` as the IPv4 destination. In the R2–Switch1 capture, Offer and ACK use `255.255.255.255`.

The R1 packet details show `192.168.1.1` as the offered client address. Compare this with the gateway address in the lab configuration. See [R2–Switch1 DHCP capture](capture-r2-switch.md).
