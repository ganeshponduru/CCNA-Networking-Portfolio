# BGP Between R7 and R9, and BGP-to-OSPF Redistribution

## Topology and router roles

This BGP connection is between R7 and R9. It is separate from the R1–R8 external BGP connection used in the earlier redistribution lab.

| Router | Role in this lab |
|---|---|
| R7 | BGP router in AS 100, connected to OSPF Area 20 (NSSA) |
| R9 | BGP peer of R7; has Loopback0 `9.9.9.9/32` |
| R3 | NSSA ABR between Area 20 and Area 0 |

R8's `8.8.8.8/32` loopback represents the simulated Google-side destination used in the earlier R1–R8 lab. It is not the BGP peer in this Day 15 R7–R9 session.

## BGP configuration visible in the lab

The R9 screenshot shows these BGP configuration lines:

```cisco
network 192.168.79.0
neighbor 192.168.79.7 remote-as 100
```

The neighbor statement identifies R7 at `192.168.79.7` as a peer in AS 100. The screenshot also shows R9's routing table learning BGP routes through `192.168.79.7`.

R9's routing table shows Loopback0 `9.9.9.9/32` as directly connected. R7's routing table shows `9.9.9.9/32` as a BGP route through `192.168.79.9`. This is consistent with R9 advertising its loopback to R7.

### R7 — peer configuration template

The R7-side neighbor command must point to R9 and use R9's actual local AS:

```cisco
configure terminal
router bgp 100
 neighbor 192.168.79.9 remote-as <R9-AS>
end
```

The supplied screenshot does not expose R9's local AS number or the complete R7 BGP configuration. Replace `<R9-AS>` with the actual AS configured on R9; do not treat this placeholder as a literal IOS command.

### R9 — route advertisement

The R9 routing table confirms that `9.9.9.9/32` is its connected Loopback0 prefix and the R7 routing table shows that prefix as BGP-learned. To originate that loopback using BGP's network statement, the configuration form is:

```cisco
configure terminal
router bgp <R9-AS>
 network 9.9.9.9 mask 255.255.255.255
end
```

This is the IOS command form consistent with the observed route. The screenshot only visibly captures `network 192.168.79.0` and the neighbor statement, so the loopback network statement above is included as the configuration needed to advertise the observed prefix, not as a command independently verified in the screenshot.

## Redistributing BGP routes into OSPF Area 20

R7 learns `9.9.9.9/32` through BGP from R9. The Area 20 LSDB shows a Type 7 AS External LSA for that prefix:

| Field | Observed value |
|---|---|
| Link State ID | `9.9.9.9` |
| Advertising Router | `7.7.7.7` (R7) |
| Route tag | `300` |

This supports the conclusion that R7, acting as an ASBR inside the NSSA, injects the BGP-learned route into OSPF as a Type 7 LSA.

The IOS command form for redistributing BGP AS 100 into OSPF process 10 on R7 is:

```cisco
configure terminal
router ospf 10
 redistribute bgp 100 subnets
end
```

The captured LSA contains route tag `300`. The provided screenshot does not show the route-map or other configuration that set this tag, so the exact tag-setting command is not asserted here.

## Type 7-to-Type 5 translation

R6 and R7 show the Type 7 LSA for `9.9.9.9` in Area 20. R3's LSDB shows the same Type 7 entry in Area 20 and also shows a Type 5 external LSA for `9.9.9.9`, advertised by Router ID `3.3.3.3` (R3), with tag `300`.

This is the observed translation performed by R3, the NSSA ABR. The translated Type 5 LSA allows the external route to be advertised into the rest of the OSPF domain.

## Verification commands

### BGP

```cisco
show ip bgp summary
show ip bgp
show ip route bgp
show running-config | section router bgp
```

### OSPF and NSSA

```cisco
show ip ospf neighbor
show ip ospf database nssa-external
show ip ospf database external
show ip route ospf
show running-config | section router ospf
```

The provided routing-table screenshot confirms BGP-learned routes on R9 and the BGP-learned `9.9.9.9/32` route on R7. It does not include `show ip bgp summary`, so the BGP Established state is not directly captured.

## Keep the two external connections separate

- **Earlier lab:** R1–R8, with R8's `8.8.8.8/32` loopback used as the simulated Google-side destination. Day 14 documents the BGP–OSPF redistribution across that connection.
- **This lab:** R7–R9, with R9's `9.9.9.9/32` loopback learned by R7 through BGP and then represented inside NSSA Area 20 by a Type 7 LSA.

These are different BGP relationships and different external prefixes. Do not describe R8 as the BGP neighbor of R7 in this lab.
