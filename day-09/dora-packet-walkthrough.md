# DHCP DORA Packet Walkthrough

## What DHCP does

Dynamic Host Configuration Protocol (DHCP) lets a client obtain IPv4 configuration from a DHCP server. Depending on the scope and options, the configuration can include an IPv4 address, subnet mask, default gateway, DNS servers, and lease timers.

DHCP uses UDP:
- Client: UDP port 68
- Server: UDP port 67

## DORA sequence

1. **Discover** — the client searches for a DHCP server. A typical initial Discover uses source IPv4 address `0.0.0.0`, destination `255.255.255.255`, and UDP ports 68 → 67.
2. **Offer** — a server proposes an address and sends available configuration options.
3. **Request** — the client indicates which offered address and server it wants to use.
4. **ACK** — the server confirms the lease and supplies the accepted configuration.

The exact IP and Ethernet destination addresses of Offer and ACK packets can vary with DHCP client behavior, BOOTP flags, server implementation, and network arrangement. Do not assume every Offer or ACK must be broadcast.

## R1–Switch1 capture

The screenshots received so far show packets 8, 10, 11, and 12 with transaction ID `0xa5338232`.

### Packet 8 — Discover

- IPv4 source: `0.0.0.0`
- IPv4 destination: `255.255.255.255`
- UDP source/destination ports: 68 → 67
- DHCP message type (option 53): Discover
- Client hardware address: `00:50:79:66:68:00`
- BOOTP flags: `0x0000` (broadcast flag not set)

### Packet 10 — Offer

- IPv4 source: `192.168.1.100`
- IPv4 destination: `192.168.1.1`
- UDP source/destination ports: 67 → 68
- DHCP message type: Offer
- Transaction ID matches the Discover: `0xa5338232`
- The `yiaddr` / “Your (client) IP address” field shows `192.168.1.1`

### Packet 11 — Request

- IPv4 source: `0.0.0.0`
- IPv4 destination: `255.255.255.255`
- UDP source/destination ports: 68 → 67
- DHCP message type: Request
- Server identifier (option 54): `192.168.1.100`
- Requested IP address (option 50): `192.168.1.1

### Packet 12 — ACK

- IPv4 source: `192.168.1.100`
- IPv4 destination: `192.168.1.1`
- UDP source/destination ports: 67 → 68
- DHCP message type: ACK
- Server identifier (option 54): `192.168.1.100`
- Subnet mask (option 1): `255.255.255.0
- Router (option 3), DNS server(s) (option 6), lease time (option 51), renewal time (option 58), and rebinding time (option 59) are present in the decoded options.

## Addressing note

The DHCP packet details show `192.168.1.1` as the offered client address. Because that address is often used as a default gateway in example networks, confirm the actual lab addressing before documenting it as both a client lease and a router address. The packet evidence alone does not tell us whether the topology intentionally uses that address for a client.




## Comparing R1–Switch1 and R2–Switch1

| Message | R1–Switch1 IPv4 destination | R2–Switch1 IPv4 destination |
|---|---|---|
| Discover | `255.255.255.255` | `255.255.255.255` |
| Offer | `192.168.1.1` | `255.255.255.255` |
| Request | `255.255.255.255` | `255.255.255.255` |
| ACK | `192.168.1.1` | `255.255.255.255` |

The screenshots show the different IPv4 destinations, but the R2 BOOTP flags and Ethernet destination MAC addresses are not visible. Without those fields, the exact reason for the broadcast replies cannot be confirmed.

See [R2–Switch1 capture analysis](capture-r2-switch.md) for the fields to verify.
