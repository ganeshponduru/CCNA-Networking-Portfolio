# DHCP DORA: Packet-Level Walkthrough

## Capture summary

The supplied Wireshark screenshots show four DHCP messages in one transaction:

| Packet | DHCP message | IPv4 source → destination | UDP source → destination | What it shows |
|---|---|---|---|---|
| 9 | Discover | `0.0.0.0 → 255.255.255.255` | `68 → 67` | Client searches for a DHCP server |
| 12 | Offer | `192.168.1.100 → 255.255.255.255` | `67 → 68` | Server offers `192.168.1.5` |
| 13 | Request | `0.0.0.0 → 255.255.255.255` | `68 → 67` | Client requests the offered address and identifies the selected server |
| 14 | ACK | `192.168.1.100 → 255.255.255.255` | `67 → 68` | Server acknowledges the allocation |

The packet list shows the same transaction ID, `0x00002242`, for all four messages. The Ethernet destination is broadcast in the displayed packets.

## 1. Discover — packet 9

Visible fields:

- Ethernet destination: `ff:ff:ff:ff:ff:ff` (broadcast)
- IPv4 source: `0.0.0.0`
- IPv4 destination: `255.255.255.255`
- IPv4 protocol: UDP (17)
- UDP source port: `68` (DHCP client)
- UDP destination port: `67` (DHCP server)
- DHCP message type, Option 53: Discover
- Transaction ID: `0x00002242`
- BOOTP message type: Boot Request (1)
- Hardware type: Ethernet (1); hardware address length: 6
- Hops: 0; seconds elapsed: 0
- Flags: `0x8000`, broadcast flag set
- Client IP address (`ciaddr`): `0.0.0.0`
- Your/client IP address (`yiaddr`): `0.0.0.0`
- Next-server IP (`siaddr`): `0.0.0.0`
- Relay-agent IP (`giaddr`): `0.0.0.0`
- Client hardware address (`chaddr`): `ca:02:1f:ec:00:1c`

The client has not yet been assigned an IPv4 address, so it uses `0.0.0.0` as the source and broadcasts the message. This is DHCP, not ARP.

Options visible include Option 53 (message type), Option 57 (maximum DHCP message size), Option 61 (client identifier), Option 12 (host name), Option 55 (parameter request list), and Option 255 (end).

## 2. Offer — packet 12

Visible fields:

- Ethernet destination: broadcast
- IPv4 source: `192.168.1.100`
- IPv4 destination: `255.255.255.255`
- UDP source port: `67`; destination port: `68`
- DHCP message type, Option 53: Offer
- Transaction ID: `0x00002242`
- Offered client address (`yiaddr`): `192.168.1.5`
- Client hardware address: `ca:02:1f:ec:00:1c`
- Server identifier, Option 54: `192.168.1.100`
- Subnet mask, Option 1: `255.255.255.0`
- Router/default gateway, Option 3: present
- DNS server, Option 6: present
- Lease time, Option 51: present
- Renewal time, Option 58: present
- Rebinding time, Option 59: present
- Option 255: end

The screenshot confirms that the options exist, but their expanded values are not all visible. Do not infer exact lease, renewal, rebinding, gateway or DNS values from this image alone.

## 3. Request — packet 13

Visible fields:

- Ethernet destination: broadcast
- IPv4 source: `0.0.0.0`
- IPv4 destination: `255.255.255.255`
- UDP source port: `68`; destination port: `67`
- DHCP message type, Option 53: Request
- Transaction ID: `0x00002242`
- Client hardware address: `ca:02:1f:ec:00:1c`
- Server identifier, Option 54: `192.168.1.100`
- Requested IP address, Option 50: `192.168.1.5`
- Options 57 (maximum message size), 61 (client identifier), 12 (host name), 55 (parameter request list), and 255 (end) are shown

The client explicitly requests `192.168.1.5` and identifies the selected server. In the initial DORA exchange, the Request is commonly broadcast so other DHCP servers can learn that this offer was selected.

## 4. ACK — packet 14

Visible fields:

- Ethernet destination: broadcast
- IPv4 source: `192.168.1.100`
- IPv4 destination: `255.255.255.255`
- UDP source port: `67`; destination port: `68`
- DHCP message type, Option 53: ACK
- Transaction ID: `0x00002242`
- Offered/acknowledged client address (`yiaddr`): `192.168.1.5`
- Server identifier, Option 54: `192.168.1.100`
- Options 51 (lease time), 58 (renewal time), 59 (rebinding time), 1 (subnet mask), 3 (router), 6 (DNS), and 255 (end) are shown

The ACK confirms the server's allocation and supplies the configuration options. A client can then configure its interface using the acknowledged values.

## Ethernet, IPv4 and UDP fields

- **Ethernet II:** the link-layer source and destination MAC addresses; broadcast destination is `ff:ff:ff:ff:ff:ff`.
- **IPv4:** source and destination addresses. The initial client messages use source `0.0.0.0`; the shown DHCP messages use destination `255.255.255.255`.
- **Protocol 17:** IPv4 identifies UDP as the next protocol.
- **UDP ports 68 and 67:** client-to-server messages use `68 → 67`; server-to-client messages use `67 → 68`.
- **DHCP transaction ID:** correlates messages belonging to the same exchange. Here it is `0x00002242`.
- **BOOTP/DHCP address fields:** `ciaddr` is the client IP address, `yiaddr` is the “your/client” address offered, `siaddr` is the next-server address, and `giaddr` is the relay-agent address.
- **`chaddr`:** client hardware address carried in the DHCP header.

## Interpretation notes

1. The four packets show a complete DORA sequence for the client in this capture.
2. DHCP server and client ports are UDP 67 and UDP 68 respectively; it is incorrect to say that the DHCP server listens on port 68.
3. Broadcast versus unicast behavior depends on the DHCP message, client state, flags and network/relay situation. These screenshots show broadcast destination addresses for the four packets.
4. The capture shows the server identifier as `192.168.1.100`; this is not a universal DHCP server address, only the value in this lab.
5. Packet capture checksums may be marked unverified by Wireshark when validation is disabled; that display alone does not prove a bad checksum.
