# DHCP Options and Header Fields

## Options visible in my capture

| Option | Name | Purpose |
|---:|---|---|
| 1 | Subnet Mask | Gives the IPv4 subnet mask |
| 3 | Router | Lists default gateway/router address(es) |
| 6 | Domain Name Server | Lists DNS resolver address(es) |
| 12 | Host Name | Client host name, when supplied |
| 50 | Requested IP Address | Address requested by the client |
| 51 | IP Address Lease Time | Lease duration |
| 53 | DHCP Message Type | Identifies Discover, Offer, Request, ACK, etc. |
| 54 | DHCP Server Identifier | Identifies the selected/offering DHCP server |
| 55 | Parameter Request List | Options the client asks the server to provide |
| 57 | Maximum DHCP Message Size | Client's advertised maximum DHCP message size |
| 58 | Renewal Time Value | T1 renewal timer |
| 59 | Rebinding Time Value | T2 rebinding timer |
| 61 | Client Identifier | Client identity value |
| 255 | End | Marks the end of the DHCP options |

Not every option appears in every DHCP message. The screenshots show some options collapsed, so their values should only be reported when expanded or otherwise readable.

## BOOTP/DHCP fixed-header fields

| Field | Meaning |
|---|---|
| `op` / Message type | Boot Request (1) or Boot Reply (2) in the BOOTP header |
| `htype` | Hardware type; 1 means Ethernet |
| `hlen` | Hardware address length; 6 for Ethernet MAC addresses |
| `hops` | Relay hop count field |
| `xid` | Transaction ID used to match messages |
| `secs` | Seconds elapsed since the client began acquisition/renewal |
| `flags` | Includes the broadcast flag |
| `ciaddr` | Client IP address, if already configured |
| `yiaddr` | “Your” client address offered/assigned by server |
| `siaddr` | Next-server IP address field |
| `giaddr` | Relay-agent IP address field |
| `chaddr` | Client hardware address |
| `sname` | Optional server host-name field |
| `file` | Optional boot-file name field |
| `options` | DHCP options, including Option 53 message type |

In the Discover screenshot, the client address fields and relay/next-server addresses are `0.0.0.0`, the transaction ID is `0x00002242`, and the client hardware address is `ca:02:1f:ec:00:1c`.

## DHCP message type values

| Option 53 value | Message |
|---:|---|
| 1 | Discover |
| 2 | Offer |
| 3 | Request |
| 4 | Decline |
| 5 | ACK |
| 6 | NAK |
| 7 | Release |
| 8 | Inform |

## Lease renewal

Two timers commonly used with DHCP leases are:

- **T1 / Option 58:** the client typically begins renewal at 50% of the lease duration, attempting to contact the original server, usually by unicast.
- **T2 / Option 59:** if renewal has not succeeded, the client typically enters rebinding at 87.5% of the lease duration and may broadcast to reach any available DHCP server.

These are standard defaults, not a guarantee that every server/client uses identical timers. The screenshots show that Options 58 and 59 are present in Offer/ACK, but do not expose their exact values.

## Important distinction

DHCP is an application-layer protocol carried over UDP/IP. It uses IPv4 and UDP to transport its messages; that does not make DHCP itself a Layer 3 protocol.
