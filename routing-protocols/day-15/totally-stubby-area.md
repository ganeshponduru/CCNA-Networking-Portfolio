# OSPF Totally Stubby Area

## What is a totally stubby area?

A totally stubby area is a Cisco extension of the stub-area concept. It blocks Type 5 external LSAs and also suppresses Type 3 inter-area summary LSAs from entering the area. The ABR advertises a default route so routers inside the area can still reach destinations outside it.

The result is a smaller LSDB and fewer specific inter-area routes inside the area.

## Difference between stub and totally stubby areas

| LSA / route behavior | Stub area | Totally stubby area |
|---|---|---|
| Type 1 Router LSA | Present | Present |
| Type 2 Network LSA | Present when applicable | Present when applicable |
| Type 3 inter-area summaries | Allowed | Suppressed, except the default summary |
| Type 4 ASBR summary | Not flooded into the area | Not flooded into the area |
| Type 5 external LSAs | Blocked | Blocked |
| Default route | Injected by ABR | Injected by ABR |

## Area 10 in this lab

- R2 is the ABR between Area 0 and Area 10.
- R4 and R5 are internal routers in Area 10.
- The captured totally-stubby LSDB shows only the default Type 3 summary route, `0.0.0.0`, advertised by R2. The other Type 3 entries seen in the earlier stub-area output are absent.
- PC1 successfully pinged `8.8.8.8` and `7.7.7.7` with five replies each.

The traceroute to `8.8.8.8` reached `192.168.18.8`, where the destination returned ICMP type 3, code 3 (destination port unreachable). The successful ping is the connectivity result; the traceroute response is not recorded as a fully successful traceroute.

## Configuration

On Cisco IOS, configure the ABR and internal routers differently.

### R2 — ABR

The `no-summary` keyword suppresses Type 3 summary LSAs into the totally stubby area. Configure it on the ABR:

```cisco
configure terminal
router ospf 10
 area 10 stub no-summary
end
```

### R4 and R5 — Internal routers

The internal routers use the regular stub command:

```cisco
configure terminal
router ospf 10
 area 10 stub
end
```

Do not configure `no-summary` on the internal routers. The ABR is responsible for suppressing the inter-area summaries and injecting the default route.

## Verification commands

```cisco
show ip ospf
show ip ospf neighbor
show ip ospf database
show ip route
show running-config | section router ospf
```

On an internal router in Area 10, verify that:

- Type 1 and applicable Type 2 LSAs remain.
- The Summary Net Link States section contains the default route `0.0.0.0`, rather than the other Type 3 inter-area prefixes.
- The routing table contains a default route via the ABR.
- External destinations remain reachable when the upstream routing is working.

## Configuration note

The ABR and all internal routers must agree on the stub-area property. The `no-summary` option is an ABR-side control; the internal routers are configured with `area 10 stub`.
