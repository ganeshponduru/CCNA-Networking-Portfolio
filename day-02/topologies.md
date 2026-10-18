# Network Topologies

These notes help me compare common physical and logical network layouts, including their trade-offs.
Topology describes how devices and links are arranged. Physical topology describes the actual cabling/layout; logical topology describes communication behavior.

| Topology | Structure | Advantages | Limitations / notes |
|---|---|---|---|
| Bus | Devices share a common backbone | Simple historical layout | Backbone failure can affect all; shared medium in classic Ethernet |
| Star | Devices connect to a central switch/hub | Easy to manage and isolate edge-link faults | Central device/uplink can be a single point of failure without redundancy |
| Ring | Each node connects to neighboring nodes in a loop | Some technologies provide alternate paths or controlled access | Recovery depends on the protocol; a broken link does not automatically guarantee recovery |
| Mesh | Nodes have multiple interconnections | Redundancy and alternate paths | Higher cabling, port, and configuration cost |
| Tree / hierarchical star | Star segments connected in a hierarchy | Scalable and common in enterprise networks | Uplinks and loops require careful design |
| Point-to-point | A link connects two endpoints | Simple, clear link ownership | Link failure disconnects endpoints unless another path exists |

Modern enterprise LANs commonly use hierarchical star designs with access, distribution, and core functions (sometimes collapsed). Redundancy, spanning-tree behavior, routing, and link aggregation influence failure handling. Do not assume a ring automatically reroutes around a failed link or that a bus topology means switches connected in a line.