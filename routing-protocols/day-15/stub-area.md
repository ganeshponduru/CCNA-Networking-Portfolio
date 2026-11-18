# OSPF Stub Area

## What is a stub area?

A stub area is an OSPF area that does not receive Type 5 AS External LSAs. These LSAs describe routes redistributed into OSPF from another routing domain, such as BGP or static routing.

Instead of carrying those external LSAs, the ABR injects a default route into the stub area. Routers inside the area can use that default route to reach destinations that are not represented by specific intra-area or inter-area routes.

A stub area still receives inter-area Type 3 Summary LSAs. This is the key difference between a stub area and a totally stubby area.

## Area 10 in this lab

- Area 10 contains R4 and R5.
- R2 connects Area 10 to the Area 0 backbone and acts as its ABR.
- The captured LSDB on R4 and R5 contains Type 1 Router LSAs, Type 2 Network LSAs, and multiple Type 3 Summary LSAs.
- The Type 3 entries include the default route as well as other inter-area prefixes.

The captured LSDB is the stub-area state before applying the totally-stubby configuration.

## Configuration

The area type must match on the ABR and all routers inside the area.

### R2 — ABR

```cisco
configure terminal
router ospf 10
 area 10 stub
end
```

### R4 and R5 — Internal area routers

```cisco
configure terminal
router ospf 10
 area 10 stub
end
```

The OSPF process ID is locally significant. The process ID shown in the Day 15 LSDB output is 10. If the local process ID differs on a router, use that router's configured process ID.

## Verification commands

```cisco
show ip ospf
show ip ospf neighbor
show ip ospf database
show ip route
show running-config | section router ospf
```

Check the following:

- The OSPF area is identified as a stub area.
- Neighbors reach the expected FULL state.
- Type 5 external LSAs are not present inside Area 10.
- Type 3 Summary LSAs for inter-area networks remain visible.
- A default route is learned from the ABR.

## Stub area vs. normal area

| Behavior | Normal area | Stub area |
|---|---|---|
| Type 1 and Type 2 LSAs | Present as applicable | Present as applicable |
| Type 3 inter-area summaries | Allowed | Allowed |
| Type 4 ASBR summary | Allowed | Not flooded into stub area |
| Type 5 external LSAs | Allowed | Blocked |
| Default route from ABR | Not required by stub behavior | Injected by ABR |

## Important configuration rule

All routers in the stub area, including the ABR, must agree that the area is a stub area. A mismatch in the stub-area setting prevents OSPF neighbor adjacency from forming across the affected link.
