# OSPF Packet Types and Header

OSPF uses five packet types to discover neighbors, form adjacencies, exchange database information, and maintain the link-state database.

## Five OSPF packet types

| Type | Packet | Purpose |
|---:|---|---|
| 1 | Hello | Discovers and maintains neighbors; carries parameters that must be compatible for neighbor formation. |
| 2 | Database Description (DBD) | Summarizes the link-state database using LSA headers during database synchronization. |
| 3 | Link State Request (LSR) | Requests specific LSAs that are missing or need a newer instance. |
| 4 | Link State Update (LSU) | Carries one or more complete LSAs to a neighbor. |
| 5 | Link State Acknowledgment (LSAck) | Acknowledges received LSAs to support reliable flooding. |

## Common OSPF header

OSPFv2 uses a 24-byte common header. Its fields are:

| Field | Description |
|---|---|
| Version | OSPF version; OSPFv2 uses version 2. |
| Type | Identifies the OSPF packet type (1–5). |
| Packet Length | Total length of the OSPF packet, including the header. |
| Router ID | Router ID of the packet sender. |
| Area ID | Area associated with the packet. |
| Checksum | Detects corruption in the OSPF packet; authentication-related handling varies by authentication type. |
| Authentication Type | Identifies the configured authentication method. |
| Authentication Data | Authentication information, depending on the authentication type. |

## Hello packet fields

A Hello packet contains information used to discover and maintain neighbors:

- Network mask
- Hello interval
- Options
- Router priority
- Dead interval
- Designated Router (DR)
- Backup Designated Router (BDR)
- Neighbor Router IDs

On the broadcast network used in the lab, the Hello interval is 10 seconds and the Dead interval is 40 seconds.

## Database Description packet fields

A DBD packet contains:

- Interface MTU
- Options
- I, M, and MS flags
- DD sequence number
- LSA headers

The flags are used during database-description exchange:

- **I (Init):** Identifies the initial DBD packet in the exchange.
- **M (More):** Indicates that more DBD packets are expected.
- **MS (Master/Slave):** Identifies the master when set; clear indicates the slave.

## Important distinction

DBD packets carry LSA headers, not the complete contents of every LSA. When a router needs a particular LSA, it sends an LSR. The neighbor responds with an LSU containing the requested LSA, and the receiver acknowledges it with an LSAck.
