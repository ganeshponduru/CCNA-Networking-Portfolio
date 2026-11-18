# BGP Between R7 and R9

## Router connection

The BGP session in this lab is between R7 and R9 over the `192.168.79.0/24` network.

- R7 is in AS 100 and has a connection to OSPF Area 20 (NSSA).
- R9 is the BGP peer. Its Loopback0 address is `9.9.9.9/32`.
- R3 is the ABR between Area 20 and Area 0.

R8 is not part of this BGP peering. Its `8.8.8.8/32` loopback was used as the simulated Google-side destination in the earlier R1–R8 lab. Keep that separate from the R7–R9 setup here.

## R9 BGP configuration shown in the screenshot

The R9 configuration output shows these commands:

```cisco
network 192.168.79.0
neighbor 192.168.79.7 remote-as 100
```

The neighbor address `192.168.79.7` is R7, and R7 is in AS 100.

R9's routing table shows BGP routes using `192.168.79.7` as the next hop. R7's routing table shows `9.9.9.9/32` as a BGP route through `192.168.79.9`. The R9 routing table shows `9.9.9.9/32` as directly connected to Loopback0.


**R7 — BGP neighbor**
```cisco
configure terminal
router bgp 100
 neighbor 192.168.79.9 remote-as 300
end
```

**R9 — advertising the loopback**

If the loopback is advertised with a BGP network statement, the command is:

```cisco
configure terminal
router bgp 300
 network 9.9.9.9 mask 255.255.255.255
end
```

## Redistributing the BGP route into OSPF

R7 learns `9.9.9.9/32` from R9 through BGP. The Area 20 LSDB then shows a Type 7 AS External LSA for that prefix, with R7's Router ID `7.7.7.7` as the Advertising Router.

The command to redistribute BGP AS 100 into OSPF process 10 on R7 is:

```cisco
configure terminal
router ospf 10
 redistribute bgp 100 subnets
end
```

This is how the BGP route can be advertised into the NSSA as a Type 7 LSA. The captured Type 7 entry has route tag `300`. The screenshot does not show how that tag was set, so no route-map configuration is assumed here.

## Type 7 translation on R3

R6 and R7 show the Type 7 LSA for `9.9.9.9` in Area 20. R3's LSDB shows the Type 7 entry in Area 20 and also lists a Type 5 LSA for `9.9.9.9`. The Type 5 entry has Advertising Router `3.3.3.3` and the same tag `300`.

This is the translation from Type 7 to Type 5 at R3, the NSSA ABR. The translated Type 5 LSA advertises the external route into the rest of the OSPF domain.

## Commands to verify BGP and OSPF

**BGP**
```cisco
show ip bgp summary
show ip bgp
show ip route bgp
show running-config | section router bgp
```

**OSPF**
```cisco
show ip ospf neighbor
show ip ospf database nssa-external
show ip ospf database external
show ip route ospf
show running-config | section router ospf
```


## Difference from the earlier R1–R8 setup

In the earlier lab, R8's `8.8.8.8/32` loopback was used as the simulated Google-side destination, with the BGP connection between R1 and R8.

In this lab, R9's `9.9.9.9/32` loopback is learned by R7 over the R7–R9 BGP connection. R7 then advertises that route into NSSA Area 20, where it appears as a Type 7 LSA.
