# OSPF Stub Area

## What is a stub area?

A stub area is used when the routers in an area do not need to know about every external network in the OSPF domain. It does not accept Type 5 external LSAs. Instead, the ABR sends a default route into the area so that routers can forward traffic for destinations they do not know about.

Type 3 Summary LSAs are still allowed in a stub area. That means routers can still learn specific routes from other OSPF areas. This is the main difference between a stub area and a totally stubby area.

## Area 10 in this lab

R4 and R5 are in Area 10, and R2 connects this area to Area 0.

In the stub-area LSDB captured before changing the area type, the routers show Type 1 Router LSAs, Type 2 Network LSAs and several Type 3 Summary LSAs. The Type 3 entries include the default route as well as routes for other OSPF areas.

## Configuration

The stub setting must be configured on the ABR and on the routers inside the area.

**R2 — ABR**
```cisco
configure terminal
router ospf 10
 area 10 stub
end
```

**R4 and R5 — Area 10 routers**
```cisco
configure terminal
router ospf 10
 area 10 stub
end
```

The OSPF process ID used in the lab outputs is 10. Remember that the process ID is locally significant, so use the process ID configured on each router.

## Commands used to check the area

```cisco
show ip ospf
show ip ospf neighbor
show ip ospf database
show ip route
show running-config | section router ospf
```

After configuring the area, check that the neighbors are up, Type 5 LSAs are not present in Area 10, and Type 3 Summary LSAs are still visible. The routing table should also contain a default route learned from the ABR.

## Stub area compared with a normal area

| LSA | Normal area | Stub area |
|---|---|---|
| Type 1 — Router | Used | Used |
| Type 2 — Network | Used when applicable | Used when applicable |
| Type 3 — Summary | Allowed | Allowed |
| Type 4 — ASBR Summary | Allowed | Not sent into the stub area |
| Type 5 — AS External | Allowed | Blocked |

The ABR injects a default route into the stub area. All routers in the area, including the ABR, must be configured for the same area type. If the stub setting does not match, OSPF neighbors will not form across the affected link.
