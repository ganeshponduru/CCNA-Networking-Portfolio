# BGP and OSPF Route Redistribution

## What is route redistribution?

Route redistribution allows routes learned by one routing protocol to be introduced into another routing protocol. It is used when a network runs more than one routing protocol and devices need to exchange reachability information between them.

In this lab, R1 connects the OSPF domain to an external BGP autonomous system.

## eBGP connection

R1 and R8 are in different autonomous systems:

| Router | Interface | Address | BGP AS |
|---|---|---|---|
| R1 | G2/0 | 192.168.18.1/24 | 100 |
| R8 | G1/0 | 192.168.18.8/24 | 200 |

The BGP neighbor relationship is configured manually:

```text
! R1
router bgp 100
 neighbor 192.168.18.8 remote-as 200

! R8
router bgp 200
 neighbor 192.168.18.1 remote-as 100
```

R8 has Loopback0 configured as 8.8.8.8/32 and advertises that prefix through BGP.

## Redistribution on R1

The lab uses both redistribution directions:

```text
router ospf 110
 redistribute bgp 100

router bgp 100
 redistribute ospf 110
```

- `redistribute bgp 100` under OSPF injects eligible BGP routes into OSPF.
- `redistribute ospf 110` under BGP injects eligible OSPF routes into BGP.

Redistribution policies, route maps, metrics, and filtering are important in production networks. The lab demonstrates the basic redistribution behavior without documenting additional policy configuration that was not shown.

## Evidence from the routing tables

### R1 learns the external prefix through BGP

Before it appears as an OSPF external route elsewhere in the domain, R1's routing table shows:

```text
B 8.8.8.8 [20/0] via 192.168.18.8
```

The route code B indicates BGP. The next hop is R8 at 192.168.18.8.

### R5 learns the external prefix through OSPF

R5's routing table shows:

```text
O E2 8.8.8.8 [110/1] via 192.168.45.4, GigabitEthernet1/0
```

This confirms the BGP-learned external prefix was redistributed into OSPF and installed as an E2 route on R5.

### R8 learns internal prefixes through BGP

The R8 routing-table capture contains BGP routes for internal router loopbacks and network prefixes with next hop 192.168.18.1 (R1). This is consistent with OSPF routes being redistributed into BGP.

## Practical distinction

Redistribution does not make BGP and OSPF become one protocol. Each protocol retains its own route-selection process and route attributes. R1 acts as the boundary where routes are injected from one routing domain into the other.

## Verification commands

```text
show ip bgp summary
show ip route
show ip route 8.8.8.8
show ip ospf database external
```
