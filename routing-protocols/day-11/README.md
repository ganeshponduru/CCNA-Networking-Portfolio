# Day 11 — Dynamic Routing and OSPF

This section introduces dynamic routing and OSPF. It covers why dynamic routing is used, how routers learn routes, how OSPF areas are organized, and how OSPF selects paths.

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
- OSPF is a link-state IGP that uses the SPF algorithm.
- OSPF routers form adjacencies, exchange link-state information, build a link-state database, and calculate routes.
- Area 0 is the OSPF backbone. A router connecting Area 0 and another area is an Area Border Router (ABR).

## Practical work

The configuration notes include the multi-area topology and commands to reproduce the OSPF setup. Practical observations and screenshots will be added after the lab outputs are available. No lab result is claimed here without a captured output.

## Verification commands

```text
show ip interface brief
show running-config | section router ospf
show ip ospf neighbor
show ip ospf database
show ip route
show ip protocols
ping <destination>
traceroute <destination>
```
