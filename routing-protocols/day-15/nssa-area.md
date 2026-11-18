# OSPF NSSA

## What is an NSSA?

A Not-So-Stubby Area (NSSA) is useful when an area should block external Type 5 LSAs coming from the rest of the OSPF domain, but still needs to bring an external route into OSPF from a router inside that area.

The router introducing the external route is the ASBR. Inside an NSSA, it advertises that route as a Type 7 LSA. The NSSA ABR can then translate the Type 7 LSA into a Type 5 LSA for the rest of the OSPF domain.

## Area 20 in this lab

R6 and R7 are in Area 20. R3 connects Area 20 to Area 0 and acts as the ABR.

The LSDB outputs show a Type 7 AS External LSA for `9.9.9.9`:

| Field | Value in the output |
|---|---|
| Link State ID | `9.9.9.9` |
| Advertising Router | `7.7.7.7` |
| Route tag | `300` |

The Type 7 entry appears in the Area 20 database on R6 and R7. R3's Area 20 database also shows it. In R3's Type 5 AS External LSA section, `9.9.9.9` appears with Advertising Router `3.3.3.3` and tag `300`. This is the Type 7-to-Type 5 translation at R3.

R3's Type 5 section also lists `8.8.8.8`, advertised by `1.1.1.1`. That is a different external route from the `9.9.9.9` route used in this NSSA example.

## Configuration

Configure Area 20 as an NSSA on the ABR and on the routers inside the area.

**R3 — ABR**
```cisco
configure terminal
router ospf 10
 area 20 nssa
end
```

**R6 and R7 — Area 20 routers**
```cisco
configure terminal
router ospf 10
 area 20 nssa
end
```

The lab output shows OSPF process ID 10. Use the process ID configured on the router if it is different.

## Redistributing the BGP route on R7

R7's routing table shows `9.9.9.9/32` as a BGP route through R9 at `192.168.79.9`. The Area 20 LSDB shows the same prefix as a Type 7 LSA advertised by R7 (Router ID `7.7.7.7`).

The IOS command to redistribute BGP AS 100 into OSPF process 10 is:

```cisco
configure terminal
router ospf 10
 redistribute bgp 100 subnets
end
```

The Type 7 and translated Type 5 LSAs show route tag `300`. The screenshots do not show the route-map or command that assigned this tag, so I have recorded the observed tag without guessing how it was configured.

## Commands to check the NSSA

```cisco
show ip ospf
show ip ospf neighbor
show ip ospf database nssa-external
show ip ospf database external
show ip route
show running-config | section router ospf
```

Look for the Type 7 entry inside Area 20 and the matching Type 5 entry outside the NSSA. The Advertising Router field helps identify the originator and the router that performed the translation.
