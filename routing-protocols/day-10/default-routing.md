# Default Routing

## Meaning

A default route is used when a router does not have a more-specific route that matches the destination IP address. It provides a next hop for traffic whose destination is not otherwise known to the routing table.

The default IPv4 route is `0.0.0.0 0.0.0.0`.

## Configuration format

```text
ip route 0.0.0.0 0.0.0.0 <next-hop-address>
```

In the Day 10 lab, R1's routing table showed:

```text
Gateway of last resort is 192.168.13.3 to network 0.0.0.0
S* 0.0.0.0/0 [10/0] via 192.168.13.3
```

This output identifies `192.168.13.3` (the Airtel router) as the next hop for the installed default route. The `S*` code identifies a candidate default static route.

## Gateway of last resort

The gateway of last resort is the next hop a router uses for packets that do not match a more-specific route, provided a usable default route is installed.

## Verification

```text
show ip route
ping 8.8.8.8
trace 8.8.8.8
```

In my lab, PC1 successfully pinged the simulated `8.8.8.8` destination. The first traceroute showed the traffic passing from R1 to Airtel.

See the [Day 10 lab overview](README.md) for the topology and screenshots.
