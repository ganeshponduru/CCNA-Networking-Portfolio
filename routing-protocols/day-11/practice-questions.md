# Dynamic Routing and OSPF — Interview and Practice Questions

## Core interview questions

### 1. What is dynamic routing?

Dynamic routing uses routing protocols to exchange routing information between routers. Routers learn remote networks, calculate preferred paths, and update their routing tables when the topology changes.

### 2. Why do we use dynamic routing instead of configuring every route manually?

Static routing becomes difficult to maintain as the number of routers and networks increases. Dynamic routing reduces manual route configuration and can adapt to topology changes.

### 3. What is an Autonomous System?

An Autonomous System (AS) is a group of networks and routers managed under a common routing policy.

### 4. What is an ASN?

An Autonomous System Number identifies an autonomous system for inter-domain routing. ASNs can be represented in 2-byte or 4-byte format.

### 5. What is the difference between IGP and BGP?

IGPs exchange routing information within an autonomous system. BGP exchanges routing information between autonomous systems and is used for inter-domain routing.

### 6. Name common IGPs.

RIP, EIGRP, OSPF, and IS-IS are common IGPs.

### 7. What is OSPF?

OSPF (Open Shortest Path First) is a link-state Interior Gateway Protocol. It uses link-state information and the Shortest Path First algorithm to calculate routes.

### 8. Which algorithm does OSPF use?

OSPF uses Dijkstra's Shortest Path First (SPF) algorithm.

### 9. What is an OSPF neighbor?

An OSPF neighbor is another OSPF-speaking router discovered through Hello packets on a shared network. Depending on the network type and conditions, routers form an adjacency and exchange link-state information.

### 10. What is an OSPF process ID?

The process ID identifies an OSPF process locally on a router. It does not have to match between neighboring routers.

### 11. What is a wildcard mask?

A wildcard mask is the inverse of a subnet mask. It tells the OSPF network command which bits of an interface IP address must match.

### 12. What is Area 0?

Area 0 is the OSPF backbone area. In a normal multi-area design, other areas connect to the backbone.

### 13. What is an ABR?

An Area Border Router (ABR) connects multiple OSPF areas and maintains area-specific link-state information. In the topology described in the notes, R2 is the ABR because it connects Area 0 and Area 10.

### 14. What is the difference between intra-area and inter-area routes?

An intra-area route is learned within the same OSPF area and is shown as `O`. An inter-area route is learned from another area and is shown as `O IA` in Cisco's IPv4 routing table.

### 15. What is OSPF administrative distance?

The default Cisco administrative distance for OSPF is 110.

### 16. What is OSPF cost?

OSPF cost is a metric used to compare paths. The total path cost is calculated by adding the relevant outgoing interface costs. Lower cost is preferred.

### 17. How does OSPF calculate interface cost?

The basic calculation is reference bandwidth divided by interface bandwidth. With the traditional 100 Mbps reference bandwidth, a 10 Mbps interface has cost 10 and a 100 Mbps interface has cost 1. A calculated value below 1 is rounded up to 1.

### 18. Does OSPF support load balancing?

Yes. OSPF can install multiple equal-cost paths to the same destination using Equal-Cost Multipath (ECMP), subject to platform limits and configuration.

## Practical and troubleshooting questions

### 19. Which command displays OSPF neighbors?

```text
show ip ospf neighbor
```

### 20. Which command displays OSPF-learned routes?

```text
show ip route ospf
```

### 21. Which command displays the OSPF configuration?

```text
show running-config | section router ospf
```

### 22. What should you check if an OSPF neighbor does not form?

Check interface status, IP addressing, subnet, area ID, OSPF enablement, Hello/dead timers, authentication, network type, and any relevant MTU or filtering issues.

### 23. What does the O route code mean?

It identifies an intra-area OSPF route.

### 24. What does O IA mean?

It identifies an inter-area OSPF route learned from another area.

### 25. How can you confirm that OSPF has learned a remote loopback?

Check the routing table with `show ip route` or `show ip route ospf`, then test reachability with ping and traceroute.

### 26. Why might a router have two next hops for one destination?

It may have two equal-cost OSPF paths and install both using ECMP.

## Quick command revision

| Purpose | Command |
|---|---|
| Check interface status | `show ip interface brief` |
| Check OSPF configuration | `show running-config | section router ospf` |
| Check OSPF neighbors | `show ip ospf neighbor` |
| Check OSPF database | `show ip ospf database` |
| Check OSPF interfaces | `show ip ospf interface` |
| Check routing table | `show ip route` |
| Show OSPF routes | `show ip route ospf` |
| Check routing protocols | `show ip protocols` |
| Test reachability | `ping <destination>` |
| Trace the path | `traceroute <destination>` |

## Important points to remember

- An OSPF process ID is locally significant; it does not need to match on neighbors.
- Area IDs must match on the shared link.
- Area 0 is the backbone.
- A router connecting Area 0 and Area 10 is an ABR.
- OSPF AD is 110 by default on Cisco routers.
- OSPF cost is a path metric, not an administrative distance.
- OSPF can use ECMP when equal-cost paths are available.
