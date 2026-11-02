# Day 10 — Default Routing

## What I am learning

A default route is used when a router does not have a more specific matching route for a destination. It provides a fallback path for traffic whose destination network is not already present in the routing table.

For IPv4, the default route is written as `0.0.0.0/0`.

## Cisco IOS command format

A common static default-route command is:

```text
ip route 0.0.0.0 0.0.0.0 <next-hop-ip-address>
```

It can also be configured using an exit interface:

```text
ip route 0.0.0.0 0.0.0.0 <exit-interface>
```

The next hop or exit interface must lead toward the intended upstream path.

## How I will verify it

```text
show ip route
show running-config
ping <destination-ip-address>
traceroute <destination-ip-address>
```

In the routing table, an IPv4 static default route is commonly displayed with the code `S*`.

## Lab notes

- **Topology:** To be added after the lab.
- **IP addressing:** To be added from the actual lab.
- **Configuration performed:** To be added after I complete the configuration.
- **Verification and observations:** To be added from my router output and test results.
- **Screenshots:** To be added after the lab.

> I will document the actual next-hop address, command output, and packet flow after completing the lab. No lab result is being assumed here.
