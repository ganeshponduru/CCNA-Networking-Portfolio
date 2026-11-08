# OSPF Database Synchronization

After neighbor discovery, OSPF routers synchronize their link-state databases so that they can calculate routes from a consistent view of the area topology.

## 1. ExStart

The routers establish the initial database-description exchange parameters. They negotiate master and slave roles and agree on the initial DBD sequence number.

## 2. Exchange

The routers send Database Description packets. These packets contain summaries of LSAs in the sender's link-state database, not the complete LSA contents.

The receiving router compares the LSA headers with its own database to identify missing or newer information.

## 3. Loading

When a router needs an LSA, it sends a Link State Request packet. The neighbor responds with a Link State Update packet containing the requested LSA. The receiving router acknowledges the LSA using a Link State Acknowledgment packet.

## 4. Full

The adjacency reaches Full when the link-state databases are synchronized. Hello packets continue to be exchanged to maintain the neighbor relationship. If the topology changes, OSPF floods updated LSAs as required.

## Packet exchange summary

| Packet | Direction / action | Role |
|---|---|---|
| DBD | Routers exchange database summaries | Identifies LSA headers and database differences |
| LSR | Router requests specific LSAs | Requests missing or newer information |
| LSU | Neighbor sends requested LSAs | Carries complete LSA information |
| LSAck | Receiver acknowledges LSAs | Confirms receipt for reliable flooding |

## Lab observations

The Wireshark capture shows R1 requesting the Router-LSA for Router ID 2.2.2.2. A Link State Update from R1 contains a Router-LSA with two links: its loopback 1.1.1.1 and the 192.168.1.0/24 network. An LSAck packet carries the header for the Router-LSA advertised by R2.

The DBD exchange capture shows R2 (2.2.2.2) with the Master bit set and R1 (1.1.1.1) with the Master bit clear. The displayed interface MTU is 1500 on both packets.
