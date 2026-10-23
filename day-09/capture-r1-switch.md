# Wireshark Capture Notes — R1 to Switch1

## Capture point

The screenshots supplied so far are from the link labelled **R1 GigabitEthernet1/0 to Switch1 Ethernet2** in Wireshark.

## Observed DHCP exchange

Transaction ID: `0xa5338232`

| Frame | Message | IPv4 source | IPv4 destination | UDP ports |
|---:|---|---|---|---|
| 8 | Discover | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 10 | Offer | `192.168.1.100` | `192.168.1.1` | 67 → 68 |
| 11 | Request | `0.0.0.0` | `255.255.255.255` | 68 → 67 |
| 12 | ACK | `192.168.1.100` | `192.168.1.1` | 67 → 68 |

The DHCP client hardware address shown is `00:50:79:66:68:00`. The Discover's BOOTP flags are `0x0000`, meaning the broadcast flag is not set in that packet.

### Packet 8 — Discover

- IPv4 source/destination: `0.0.0.0` → `255.255.255.255`
- UDP ports: 68 → 67
- DHCP message type (option 53): Discover
- Client hardware address: `00:50:79:66:68:00)
- Transaction ID: `0xa5338232`

### Packet 10 — Offer

- IPv4 source/destination: `192.168.1.100` → `192.168.1.1`
- UDP ports: 67 → 68
- DHCP message type: Offer
- “Your (client) IP address” (`yiaddr`): `192.168.1.1`
- Transaction ID matches the Discover: `0xa5338232`

### Packet 11 — Request

- IPv4 source/destination: `0.0.0.0` → `255.255.255.255`
- UDP ports: 68 → 67
- DHCP message type: Request
- Server identifier (option 54): `192.168.1.100`
- Requested IP address (option 50): `192.168.1.1`
- Transaction ID: `0xa5338232`

### Packet 12 — ACK

- IPv4 source/destination: `192.168.1.100` → `192.168.1.1`
- UDP ports: 67 → 68
- DHCP message type: ACK
- Server identifier (option 54): `192.168.1.100`
- Subnet mask (option 1): `255.255.255.0`
- Router (option 3), DNS server(s) (option 6), lease time (option 51), renewal time (option 58), and rebinding time (option 59) are present in the decoded options.

## Interpretation and items to verify

In this capture, Offer and ACK use `192.168.1.1` as their IPv4 destination, while Discover and Request use the limited broadcast address `255.255.255.255`.

The destination address alone does not explain why a second capture shows broadcast replies. To compare the R2–Switch capture, inspect:
1. BOOTP broadcast flag in its Discover and Request packets
2. IPv4 destination and Ethernet destination of Offer and ACK
3. Client address (`ciaddr`), offered address (`yiaddr`), and client hardware address (`chaddr`)
4. Server identifier and transaction ID

**Addressing check:** `192.168.1.1` is shown as the offered client address. Verify that this does not conflict with the gateway address in the intended lab topology before making a final claim about the configuration.
