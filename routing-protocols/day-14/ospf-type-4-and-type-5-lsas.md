# OSPF Type 4 and Type 5 LSAs

## Why both LSAs are needed

When an external route is redistributed into OSPF, routers need two pieces of information:

- The external destination being advertised.
- A way to reach the ASBR that originated the external route.

OSPF uses Type 5 and Type 4 LSAs for these purposes.

## Type 5 – AS External LSA

A Type 5 LSA is originated by an ASBR to advertise an external prefix into the OSPF domain.

In this lab:

| Field | Observed value |
|---|---|
| Link State ID | 8.8.8.8 |
| Advertising Router | 1.1.1.1 (R1) |
| LSA type | Type 5 – AS External |

R1 originates the Type 5 LSA for the external test prefix 8.8.8.8/32 after redistributing the BGP route into OSPF.

Type 5 LSAs have AS-wide flooding scope in a normal OSPF domain. They are not flooded into stub areas. NSSA uses Type 7 LSAs for external routes originated inside the NSSA.

## Type 4 – ASBR Summary LSA

A Type 4 LSA is originated by an ABR to advertise reachability information for an ASBR into another area. Its Link State ID identifies the ASBR's Router ID, while the Advertising Router identifies the ABR that originated the Type 4 LSA.

The lab database captures show:

| Receiving router | Area | Link State ID (ASBR) | Advertising Router (ABR) |
|---|---:|---|---|
| R5 | 10 | 1.1.1.1 (R1) | 2.2.2.2 (R2) |
| R6 | 20 | 1.1.1.1 (R1) | 3.3.3.3 (R3) |

R2 advertises the ASBR reachability information into Area 10. R3 advertises it into Area 20.

## Type 1 Router-LSA role flags

The Wireshark capture shows the role flags in Type 1 Router-LSAs:

| Router | B flag (ABR) | E flag (ASBR) | Role shown |
|---|---|---|---|
| R1 (1.1.1.1) | Clear | Set | ASBR |
| R2 (2.2.2.2) | Set | Clear | ABR |

The B flag indicates Area Border Router. The E flag indicates AS Boundary Router. These flags are separate: a router can be an ABR, an ASBR, both, or neither depending on its roles.

## How the LSAs work together

1. R1 originates a Type 5 LSA describing the external prefix 8.8.8.8/32.
2. An internal router in another area also needs a route toward the ASBR R1.
3. The ABR originates a Type 4 LSA identifying R1 as the ASBR and advertising reachability toward it.
4. The receiving router uses the external route information and the path to the ASBR to calculate the route.

The Type 4 LSA does not carry the external destination prefix. The Type 5 LSA carries that prefix.

## Verification commands

```text
show ip ospf database
show ip ospf database asbr-summary
show ip ospf database external
show ip ospf database router
```
