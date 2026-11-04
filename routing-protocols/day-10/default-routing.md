# Default Routing

## What is a default route?

A default route is used when a router does not have a more-specific matching route for a destination. It provides a next hop for traffic going to destinations that are not otherwise listed in the routing table.

The IPv4 default route is:

```text
0.0.0.0 0.0.0.0
```

It is also written as `0.0.0.0/0`.

## Default route command

```text
Router(config)# ip route 0.0.0.0 0.0.0.0 <next-hop-ip>
```

Example:

```text
Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.2
```

This tells the router to forward packets that have no more-specific matching route to next hop `192.168.1.2`.

## Gateway of last resort

The gateway of last resort is the next hop used for destinations that do not match a more-specific route, when a usable default route is installed.

Use this command to check the routing table:

```text
show ip route
```

A static default route is marked with `S*` in the Cisco IPv4 routing table:

- `S` means the route is static.
- `*` marks it as a candidate default route.
- `0.0.0.0/0` is the default prefix.

## Administrative distance in the Day 10 lab

The default administrative distance of a static route is **1**. However, the route shown in the captured R1 routing table was configured with AD **10**:

```text
Gateway of last resort is 192.168.13.3 to network 0.0.0.0
S* 0.0.0.0/0 [10/0] via 192.168.13.3
```

In `[10/0]`, `10` is the administrative distance and `0` is the metric. The next hop `192.168.13.3` is the Airtel-facing router in the recorded lab topology.

The class handout also gives an example of a second default route with a higher AD for the Tata path. A higher AD makes that route less preferred while the lower-AD route is available.

## Primary and backup ISP paths

The lab scenario uses two ISP paths:

- **Airtel:** preferred path.
- **Tata:** backup path.

The office router has a default route towards the preferred ISP and a floating static default route towards the backup ISP. When the preferred path is no longer usable, the backup route can be selected if it remains reachable.

## Verification in the lab

The recorded checks were:

```text
show ip route
ping 8.8.8.8
trace 8.8.8.8
```

The R1 routing-table output showed the installed default route through Airtel. The initial PC traceroute also showed the Airtel next hop. After the Airtel-facing interface was shut down, the later traceroute showed the Tata path.

See the [Day 10 lab overview](README.md) and [floating static route and failover notes](floating-static-route-and-failover.md) for the screenshots and path observations.

## Important distinction about 8.8.8.8

In this lab, `8.8.8.8` is configured as a loopback address on R5 to represent the destination. A successful ping verifies reachability to that simulated destination inside the lab. It does **not** by itself prove that the PC has access to the public Internet or that NAT is configured.

## Quick comparison

| Static route | Default route |
|---|---|
| Points to a specific destination network or host | Matches destinations without a more-specific route |
| Uses the destination network and mask | Uses `0.0.0.0 0.0.0.0` |
| Useful when the destination is known | Commonly used as an exit path towards an ISP or upstream router |
