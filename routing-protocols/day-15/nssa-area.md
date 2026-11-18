# OSPF Not-So-Stubby Area (NSSA)

## What is an NSSA?

A Not-So-Stubby Area (NSSA) is a special OSPF area that blocks Type 5 external LSAs from entering the area, like a stub area, but still allows an ASBR inside the area to introduce external routes.

An ASBR inside an NSSA originates Type 7 AS External LSAs for the redistributed routes. The NSSA ABR can translate Type 7 LSAs into Type 5 LSAs so the external routes can be advertised into the rest of the OSPF domain.

## Type 7 and Type 5 LSA roles

| LSA type | Purpose | Where it is used |
|---|---|---|
| Type 7 | Advertises an external route originated inside an NSSA | Within the NSSA |
| Type 5 | Advertises an external route across the normal OSPF domain | Outside stub/NSSA restrictions |

The Type 7 LSA identifies the external prefix and the NSSA ASBR that originated it. The NSSA ABR performs Type 7-to-Type 5 translation for advertisement outside the NSSA.

## Area 20 observations in this lab

The captured LSDB outputs show the following Type 7 entry in Area 20:

| Field | Observed value |
|---|---|
| Link State ID | `9.9.9.9` |
| Advertising Router | `7.7.7.7` |
| Route tag | `300` |

The Type 7 LSA is visible in the Area 20 LSDB on R6 and R7 and in the Area 20 database displayed on R3.

R3's Type 5 AS External LSA section also contains `9.9.9.9`, advertised by Router ID `3.3.3.3`, with tag `300`. This is the observed Type 7-to-Type 5 translation at R3.

The same Type 5 section contains `8.8.8.8`, advertised by `1.1.1.1`. That is a separate external route and is not the translated `9.9.9.9` entry.

## Configuration

The following commands show the area-type configuration for the topology. Apply the NSSA setting consistently on the ABR and all routers inside Area 20.

### R3 — ABR between Area 0 and Area 20

```cisco
configure terminal
router ospf 10
 area 20 nssa
end
```

### R6 and R7 — Routers inside Area 20

```cisco
configure terminal
router ospf 10
 area 20 nssa
end
```

The OSPF process ID shown in the Day 15 database output is 10. Use the actual local process ID if it differs.

## Redistributing an external route into the NSSA

The Type 7 LSA observed for `9.9.9.9` is advertised by R7 (Router ID `7.7.7.7`). R7's routing table shows `9.9.9.9/32` as a BGP-learned route via `192.168.79.9`.

If the lab configuration redistributes BGP into OSPF on R7, the IOS command form is:

```cisco
configure terminal
router ospf 10
 redistribute bgp 100 subnets
end
```

The route tag `300` is visible in the captured Type 7 and translated Type 5 LSAs. The screenshot proves the tag value, but does not show the exact route-map or redistribution command used to set it. Do not claim a specific tag-setting command unless it is confirmed in the running configuration.

## Verification commands

```cisco
show ip ospf
show ip ospf neighbor
show ip ospf database nssa-external
show ip ospf database external
show ip route
show running-config | section router ospf
```

Check that:

- The Area 20 routers agree on NSSA configuration.
- The external route appears as a Type 7 LSA inside Area 20.
- The NSSA ABR translates the Type 7 route into a Type 5 LSA for the rest of the OSPF domain.
- The translated Type 5 LSA retains the expected external prefix and route tag.

## Important distinction

An NSSA does not simply allow ordinary Type 5 LSAs to flood into the area. It provides a controlled way for external routes originating inside the area to be represented as Type 7 LSAs and then translated by the ABR.
