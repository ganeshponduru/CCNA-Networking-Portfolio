# Day 11 — Dynamic Routing and OSPF

This section covers dynamic routing, routing protocol types, autonomous systems, OSPF operation, multi-area design, route selection, and verification.

## Topics and notes

| Topic | Notes |
|---|---|
| Dynamic routing | [Dynamic routing fundamentals and routing protocol types](dynamic-routing-fundamentals.md) |
| OSPF configuration | [OSPF configuration, wildcard masks, and multi-area design](ospf-multi-area-configuration.md) |
| OSPF path selection | [OSPF cost, route types, and equal-cost load balancing](ospf-cost-and-load-balancing.md) |
| Revision | [Interview and practice questions](practice-questions.md) |

## Main concepts

- Dynamic routing protocols exchange routing information and learn remote networks automatically.
- IGPs operate within an autonomous system; BGP is used to exchange routing information between autonomous systems.
- OSPF is a link-state IGP that uses the Shortest Path First (SPF) algorithm.
- OSPF routers discover neighbors, form adjacencies, exchange link-state information, build a link-state database, and calculate routes.
- Area 0 is the OSPF backbone. A router connecting Area 0 and another area is an Area Border Router (ABR).

## Practical topology

The lab uses five routers. R1, R2, R3, and R4 form the Area 0 portion of the topology. R2 also connects to R5 through the 192.168.25.0/24 network, which is assigned to Area 10. R2 therefore acts as the ABR between Area 0 and Area 10.

![OSPF lab topology](Screenshot%202026-11-04%20214229.png)

## OSPF neighbor verification

The show ip ospf neighbor output on R2 lists three neighbors:

| Neighbor ID | Neighbor address | Interface on R2 | Observed state |
|---|---|---|---|
| 4.4.4.4 | 192.168.24.4 | GigabitEthernet1/0 | FULL/DR |
| 3.3.3.3 | 192.168.23.3 | GigabitEthernet2/0 | FULL/DR |
| 5.5.5.5 | 192.168.25.5 | Ethernet3/0 | FULL/DR |

All three displayed adjacencies reached the FULL state. The neighbor output confirms that R2 formed OSPF adjacencies with R3, R4, and R5.

![R2 OSPF routing table and neighbor states](Screenshot%202026-11-04%20215258.png)

## Routing-table observations

The router outputs show OSPF-learned routes marked with O and inter-area routes marked with O IA.

- R3 learns remote loopbacks and networks through its OSPF neighbors.
- R4 learns remote loopbacks and networks through its OSPF neighbors.
- R2 has an inter-area route to the 5.5.5.5/32 loopback through R5 at 192.168.25.5.
- R5 learns the Area 0 loopbacks and networks through R2 at 192.168.25.2.
- OSPF routes in the displayed routing tables use administrative distance 110.

![R1 and R5 routing tables with ping test](Screenshot%202026-11-04%20214440.png)

![R3 and R4 OSPF routing tables](Screenshot%202026-11-04%20215138.png)

## End-to-end connectivity test

From R1, the destination 5.5.5.5 was tested using ping.

- The first test showed 2 successful replies out of 5 (40%). OSPF neighbor state-change messages appeared during this period, indicating that adjacencies were still transitioning to FULL.
- A subsequent ping test showed 5 successful replies out of 5 (100%).

This demonstrates successful reachability from R1 to R5's loopback after OSPF convergence in the lab.

## Verification commands

```text
show ip interface brief
show running-config | section router ospf
show ip ospf neighbor
show ip ospf database
show ip route
show ip protocols
ping 5.5.5.5
traceroute 5.5.5.5
```

## Load balancing

OSPF cost and equal-cost paths are covered in [OSPF cost and load balancing](ospf-cost-and-load-balancing.md). The additional load-balancing screenshots and observations will be added after that part of the lab is completed and captured.
