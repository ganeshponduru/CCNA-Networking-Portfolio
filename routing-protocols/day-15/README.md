# OSPF Stub Areas, Totally Stubby Areas, and NSSA

This folder is reserved for the next OSPF lab covering stub area types.

## Topics

- Stub area
- Totally stubby area
- Not-so-stubby area (NSSA)

## Topology overview

The topology is divided into the following OSPF areas and connected networks:

| Area / network | Routers and devices | Purpose |
|---|---|---|
| Area 0 | R1, R2, R3 | OSPF backbone |
| Area 10 | R4, R5 | Labelled as a totally stubby area |
| Area 20 | R6, R7 | Labelled as an NSSA |
| External connection | R1–R8 | External router connection |
| PC1/PC2 LAN | R5 and Switch1 | End devices connected on the Area 10 side |
| PC3 LAN | R7–R9 | End device connected on the Area 20 side through R9 |

The topology labels Area 10 as **totally stubby** and Area 20 as **NSSA**. These are the intended area types for the lab; the actual router configurations and verification outputs will be documented after they are performed.

## Lab work

The configuration and verification results will be added after completing the lab. No configuration or practical result is documented here yet.

## Planned verification

- Area configuration on the relevant routers
- OSPF neighbor status
- Routing-table comparison inside and outside each area
- LSDB comparison, including the LSA types allowed or blocked in each area
- Default-route behavior in stub area types
- NSSA Type 7 LSA behavior and translation, if demonstrated in the lab
