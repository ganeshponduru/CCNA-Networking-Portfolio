# DHCP Options and BOOTP Header Fields

## Common DHCP options

| Option | Name | Purpose |
|---:|---|---|
| 1 | Subnet Mask | Specifies the client's IPv4 subnet mask |
| 3 | Router | Supplies default-gateway address(es) |
| 6 | Domain Name Server | Supplies DNS server address(es) |
| 12 | Host Name | Client host name, when supplied |
| 50 | Requested IP Address | Address requested by the client |
| 51 | IP Address Lease Time | Requested/granted lease duration |
| 53 | DHCP Message Type | Identifies Discover, Offer, Request, ACK, and other DHCP message types |
| 54 | DHCP Server Identifier | Identifies the DHCP server |
| 55 | Parameter Request List | Lists configuration parameters the client requests |
| 57 | Maximum DHCP Message Size | Indicates the client's maximum DHCP message size |
| 58 | Renewal Time Value | T1 timer for lease renewal |
| 59 | Rebinding Time Value | T2 timer for rebinding |
| 61 | Client Identifier | Client identifier, when present |
| 255 | End | Marks the end of the options |

Not every packet contains every option. The options depend on the message and the client/server exchange.

## DHCP/BOOTP message fields

| Field | Meaning |
|---|---|
| `op` | Operation: request or reply |
| `htype` | Hardware type, such as Ethernet |
| `hlen` | Hardware address length |
| `hops` | Relay hop count |
| `xid` | Transaction ID used to match messages in an exchange |
| `secs` | Seconds elapsed since the client began address acquisition |
| `flags` | Includes the broadcast flag |
| `ciaddr` | Client IP address, when already known/configured in the relevant state |
| `yiaddr` | “Your” client IP address offered/assigned by the server |
| `siaddr` | Next-server address field used in BOOTP/DHCP contexts |
| `giaddr` | Relay-agent IP address, used when a relay is involved |
| `chaddr` | Client hardware address |
| `sname` | Optional server host-name field |
| `file` | Optional boot-file name field |
| `options` | DHCP options, including message type and configuration parameters |

## Timers and lease behavior

T1 is commonly set to 50% of the lease duration and T2 commonly to 87.5%, but these are typical defaults, not universal values. At T1, the client normally attempts renewal with the original server. If renewal fails and T2 is reached, it can attempt rebinding with any available DHCP server. Exact behavior depends on protocol state and configuration.

## Reading packet fields carefully

- UDP ports identify the DHCP client/server transport endpoints: 68 and 67.
- Option 53 identifies the DHCP message type.
- Option 54 identifies the DHCP server.
- Option 50 in a DHCP Request can show the address the client is requesting.
- `yiaddr` in an Offer/ACK is the address the server is offering or acknowledging for the client.
- The BOOTP broadcast flag is not the same thing as the IPv4 destination address. Inspect both the flag and the IP/Ethernet destination fields when comparing captures.
