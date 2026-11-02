# Day 10 — Default Routing

## What I learned

A default route is used when a router does not have a more specific matching route for the destination. For IPv4, the default route is `0.0.0.0/0`.

Cisco IOS command format:

```text
ip route 0.0.0.0 0.0.0.0 <next-hop-ip-address>
```

The route can also be configured with an exit interface when appropriate.

## Default route in this lab

R1 is connected to two upstream routers:

- R3 (Airtel): `192.168.13.3`
- R4 (Tata): `192.168.14.4`

The captured R1 routing table displays:

```text
Gateway of last resort is 192.168.13.3 to network 0.0.0.0

S* 0.0.0.0/0 [10/0] via 192.168.13.3
```

This confirms that R1's installed default route points to R3 (Airtel), with administrative distance 10.

**Important:** The supplied R1 screenshot shows only this one installed default route. It does not show a second default route via R4, so I cannot claim that floating-static-route failover was verified on R1 from these screenshots.

## Connectivity test from PC1

The PC1 terminal shows:

- The first ping attempt to `8.8.8.8` received timeouts.
- A subsequent ping attempt received five replies from `8.8.8.8`.
- The successful replies had TTL 253.
- Traceroute displayed these hops:

```text
1  192.168.1.100
2  192.168.13.3
3  192.168.35.5
```

The final traceroute response was ICMP type 3, code 3 (destination port unreachable). The successful ping replies independently confirm that PC1 could reach `8.8.8.8` during the later test.

## What is not yet verified

The topology includes both Airtel and Tata paths, but the captured R1 routing table shows only the Airtel default route. To demonstrate floating static default-route failover, the lab would need to show:

1. A second default route via R4 with a higher administrative distance.
2. The R1 routing table before the primary path is removed.
3. The R1 routing table after the primary path becomes unavailable.
4. A successful connectivity test through R4 after failover.

A higher-AD backup route alone does not prove that failures beyond the directly connected next hop will be detected. The failure-detection method also matters.

## Commands used for verification

```text
show ip route
ping 8.8.8.8
traceroute 8.8.8.8
```
