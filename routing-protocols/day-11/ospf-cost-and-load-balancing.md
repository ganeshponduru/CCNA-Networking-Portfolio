# OSPF Cost, Route Types, and Equal-Cost Load Balancing

## OSPF route codes

Common OSPF route codes in the Cisco IPv4 routing table include:

| Code | Meaning |
|---|---|
| O | Intra-area OSPF route |
| O IA | Inter-area OSPF route |
| O E1 / O E2 | OSPF external routes, Type 1 or Type 2 |
| O N1 / O N2 | OSPF NSSA external routes, Type 1 or Type 2 |

A route marked `O` is learned within the same OSPF area. A route marked `O IA` is learned from another OSPF area. For example, a router in Area 0 can learn the 5.5.5.5/32 loopback from Area 10 as an inter-area route through the ABR.

## Administrative distance and OSPF cost

Administrative Distance (AD) indicates the preference of a route source when a router has competing routes to the same destination prefix. A lower AD is preferred.

The default Cisco administrative distance for OSPF is **110**.

OSPF cost is a metric used to compare paths within OSPF. Do not confuse the two:

- AD compares route sources for the same destination prefix.
- OSPF cost compares paths learned by OSPF.
- Longest-prefix match is used when forwarding a packet among matching destination prefixes.

## OSPF cost calculation

On Cisco IOS, the default reference bandwidth on many classic configurations is 100 Mbps. The basic cost calculation is:

```text
OSPF cost = reference bandwidth / interface bandwidth
```

Using the traditional 100 Mbps reference bandwidth:

| Interface bandwidth | Calculated value | Typical default cost |
|---|---:|---:|
| 10 Mbps | 100 / 10 | 10 |
| 100 Mbps | 100 / 100 | 1 |
| 1 Gbps | 100 / 1000 | 0.1, rounded up to 1 |

OSPF cost is an integer, so a calculated value below 1 is treated as 1. The reference bandwidth can be changed; in a modern network, it should be configured consistently across routers so faster links can be differentiated.

The total path cost is the sum of the outgoing interface costs along the path to the destination. A lower total cost is preferred.

### Example path calculation

Suppose R1 can reach R5 through two paths:

- Path 1: R1 → R3 → R2 → R5
- Path 2: R1 → R4 → R2 → R5

If each path has a total OSPF cost of 13, the paths are equal-cost paths.

The router can install both paths in the routing table and forward traffic across them, subject to platform and forwarding behavior.

## Equal-cost multipath (ECMP)

OSPF supports equal-cost multipath. When multiple paths to the same destination have the same best OSPF cost, a router can install multiple next hops, up to the platform's configured maximum-path limit.

This is called **Equal-Cost Multipath (ECMP)**. It can distribute traffic over more than one next hop. The exact distribution method depends on the platform and forwarding implementation; it is not necessarily a simple one-packet-at-a-time split.

Use these commands to inspect routes and paths:

```text
show ip route
show ip route ospf
show ip ospf interface
show ip protocols
traceroute <destination>
```

A routing table that displays multiple next hops for the same OSPF destination is evidence that multiple paths have been installed. A traceroute may show different paths across probes, but one traceroute alone does not prove equal traffic distribution for all applications or flows.

## Useful troubleshooting checks

- Compare the OSPF cost on each interface in the candidate paths.
- Confirm that the total path costs are equal if ECMP is expected.
- Check whether the platform's maximum-path setting permits multiple next hops.
- Confirm the routing table actually contains multiple next hops.
- Use suitable traffic tests and platform-specific commands to examine forwarding distribution.
