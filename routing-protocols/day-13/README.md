# OSPF Multi-Area LSA Database Analysis

This lab focuses on examining the OSPF link-state database across Areas 0, 10, and 20. The main purpose is to identify Type 1, Type 2, and Type 3 LSAs and understand how their contents differ between areas.

## Lab scope

- Inspect the OSPF database on R1, R2, R3, R5, and R7.
- Identify Router LSAs, Network LSAs, and Summary Network LSAs.
- Compare the local-area information with inter-area prefixes.
- Identify the ABRs from the actual interface area assignments.

Connectivity was already verified in the lab. This documentation focuses on the LSA database rather than repeating DHCP, ping, or traceroute tests.

## Topology and area roles

The multi-area design contains:

| Area | Routers |
|---|---|
| Area 0 | R1, R2, R3, R4 |
| Area 10 | R4, R5 |
| Area 20 | R3, R6, R7 |

Based on the interface-area assignments used in this run:

- **R3** connects Area 0 and Area 20 and operates as an ABR.
- **R4** connects Area 0 and Area 10 and operates as an ABR.
- **R2** remains an internal router in Area 0. Its G2/0 interface was deliberately assigned to Area 0, so R2 does not act as an ABR in this configuration.

## Captured outputs

The screenshots reviewed for this analysis cover:

- OSPF database output from R1, R5, and R7.
- OSPF database output from R2 and R3.
- OSPF interface-area assignments on R2, R4, and R5.

The observations below are based on those captures. The image files themselves still need to be added to the repository; no broken image links are included.

## Main observations

- Type 1 Router LSAs describe a router's links within an area. They are not carried unchanged from one area into another.
- Type 2 Network LSAs describe a broadcast or NBMA multi-access network and are originated by its DR.
- Type 3 Summary LSAs are originated by ABRs to advertise inter-area network prefixes.
- The LSA database is area-scoped. A router's database contains the LSAs relevant to the areas in which it participates, along with inter-area summary information.
- In this configuration, R3 and R4 are the ABRs. R2 is not an ABR because both of its shown OSPF interfaces are assigned to Area 0.

## Configuration experiment

The R2–R4 link was intentionally kept in Area 0 on both ends instead of assigning it to Area 10 as in the reference lab design. This was done to observe how OSPF area membership affects ABR identification.

With this assignment, R2 remains an internal Area 0 router and R4 becomes the ABR for Area 10. This is a valid multi-area arrangement as long as the interfaces at both ends of each OSPF link agree on the area.

## Important clarification

An ABR is identified by its participation in multiple OSPF areas, normally including Area 0. A router does not become an ABR merely because it is physically positioned between two routers or because a link connects to another area. The OSPF area configured on its interfaces determines its area participation.

An ABR does not literally convert Type 1 and Type 2 LSAs into Type 3 LSAs. It originates Type 3 Summary LSAs to advertise inter-area prefixes.

## Verification command

```text
show ip ospf database
show ip ospf interface brief
```
