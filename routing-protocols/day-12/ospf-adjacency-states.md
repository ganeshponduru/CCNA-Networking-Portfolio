# OSPF Neighbor Adjacency States

OSPF neighbors move through states as they discover one another and synchronize their link-state databases.

## Neighbor states

| State | Description |
|---|---|
| Down | No Hello packet has been received from the neighbor within the relevant interval. |
| Attempt | Used on NBMA networks when a neighbor is configured statically; the router is actively trying to contact it. |
| Init | A Hello packet has been received, but the local Router ID is not yet listed in the neighbor's Hello packet. |
| 2-Way | Bidirectional Hello communication is confirmed because each router sees the other's Router ID in its Hello packet. |
| ExStart | Routers negotiate the master/slave relationship and initial database-description sequence numbers. |
| Exchange | Routers exchange DBD packets containing summaries of their LSDBs. |
| Loading | Routers request missing or newer LSAs with LSR packets and receive them in LSU packets. |
| Full | The adjacent routers have synchronized their link-state databases. |

There are eight named states when Attempt is included. Attempt is specific to NBMA neighbor discovery; the usual progression on broadcast and point-to-point networks does not use it.

## Typical state progression

```text
Down
  |
 Init
  |
2-Way
  |
ExStart
  |
Exchange
  |
Loading
  |
 Full
```

The progression can vary by network type and adjacency requirements. On broadcast networks, two DROTHER routers can remain in the 2-Way state with each other; they do not need to form a Full adjacency with every router.

## Master and slave roles

During ExStart, routers negotiate which one will control the DBD exchange. The router with the higher Router ID becomes the master when the negotiation begins normally. The master controls the DBD sequence numbers; the slave responds using the negotiated sequence.

## DR and BDR election

On multiaccess networks such as Ethernet broadcast segments, OSPF elects a Designated Router and Backup Designated Router to reduce the number of required adjacencies and organize LSA exchange.

Election order:

1. Highest OSPF interface priority wins.
2. If priorities are equal, the highest Router ID wins.
3. An interface with priority 0 is not eligible to become DR or BDR.

DR/BDR election is separate from the DBD master/slave negotiation. Do not confuse the DR role with the DBD master role.

## MTU mismatch

An MTU mismatch can prevent two OSPF routers from completing adjacency formation, commonly leaving the neighbor stuck around ExStart or Exchange. The exact symptoms depend on the platform and configuration. Verify interface MTUs at both ends when adjacency formation does not progress.
