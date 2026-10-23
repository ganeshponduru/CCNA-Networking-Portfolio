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

The DHCP client hardware address shown is `00:50:79:66:68:00`. The Discover's BOOTP broadcast flag is `0x0000`, so the broadcast flag is not set in that packet.

The Request includes:
- Option 53: DHCP Request
- Option 54: DHCP Server Identifier = `192.168.1.100`
- Option 50: Requested IP Address = `192.168.1.1)
- Option 61: Client Identifier
- Option 12: Host Name
- Option 55: Parameter Request List

The ACK includes:
- Option 53: DHCP ACK
- Option 54: DHCP Server Identifier = `192.168.1.100`
- Option 51: IP Address Lease Time
- Option 58: Renewal Time Value
- Option 59: Rebinding Time Value
- Option 1: Subnet Mask = `255.255.255.0`
- Option 3: Router
- Option 6: Domain Name Server

## Interpretation and items to verify

The Offer and ACK use the offered client address `192.168.1.1` as their IPv4 destination in this capture, whereas Discover and Request use the limited broadcast address `255.255.255.255`.

Before drawing a conclusion about why a different capture point shows broadcast Offer/ACK packets, compare the second capture's:
1. DHCP BOOTP broadcast flag in Discover and Request
2. IPv4 destination and Ethernet destination of Offer and ACK
3. Client address (`ciaddr`), offered address (`yiaddr`), and client hardware address (`chaddr`)
4. Server identifier and transaction ID

The second capture has not been added yet. No conclusion about its cause is recorded here.
