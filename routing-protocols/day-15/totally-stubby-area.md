# OSPF Totally Stubby Area

## What changes in a totally stubby area?

A totally stubby area takes the stub-area restriction one step further. It blocks Type 5 external LSAs and also stops the normal Type 3 inter-area summary LSAs from entering the area. The ABR still sends a default route, so routers inside the area can reach networks outside it without keeping all those individual routes.

On Cisco IOS, this is configured with the `no-summary` keyword on the ABR.

## Area 10 in this lab

R2 is the ABR for Area 10, and R4 and R5 are the routers inside the area.

In the first LSDB capture, from the stub-area setup, several Type 3 Summary LSAs are present. After changing Area 10 to totally stubby, the later LSDB capture shows only the default Type 3 summary route, `0.0.0.0`, advertised by R2. The other Type 3 entries seen earlier are no longer listed.

PC1 also passed the connectivity test: it received five successful replies when pinging `8.8.8.8` and `7.7.7.7`.

The traceroute to `8.8.8.8` reached `192.168.18.8`, but the destination returned ICMP type 3, code 3 (destination port unreachable). So the ping test succeeded; the traceroute did not complete normally.

## Configuration

The ABR and the internal routers do not use exactly the same command.

**R2 — ABR**

```cisco
configure terminal
router ospf 10
 area 10 stub no-summary
end
```

**R4 and R5 — routers inside Area 10**

```cisco
configure terminal
router ospf 10
 area 10 stub
end
```

Use `no-summary` on the ABR. The internal routers use the normal `area 10 stub` command.

## Commands to verify the result

```cisco
show ip ospf
show ip ospf neighbor
show ip ospf database
show ip route
show running-config | section router ospf
```

On R4 or R5, check the Summary Net Link States section. The default summary route should remain, while the other Type 3 inter-area summaries should be absent. Also check that the routing table has a default route through the ABR.

## Stub vs. totally stubby

| LSA / route | Stub area | Totally stubby area |
|---|---|---|
| Type 1 and Type 2 | Present as applicable | Present as applicable |
| Type 3 inter-area summaries | Allowed | Suppressed, except the default route |
| Type 4 and Type 5 | Not sent into the area | Not sent into the area |
| Default route | Injected by ABR | Injected by ABR |

The important point from this lab is the change in the Type 3 entries. A stub area still has inter-area summaries; a totally stubby area keeps the default summary instead.
