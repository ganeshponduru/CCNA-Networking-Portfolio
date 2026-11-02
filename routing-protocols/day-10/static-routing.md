# Day 10 — Static Routing

## What I am learning

Static routing is a method where a network administrator manually adds routes to a router's routing table. The router does not learn these routes automatically from another routing protocol.

A static route is useful when the network topology is small or when a specific path should be configured manually.

## Cisco IOS command format

```text
ip route <destination-network> <subnet-mask> <next-hop-ip-address>
```

A route can also be configured using an exit interface, depending on the interface type and topology:

```text
ip route <destination-network> <subnet-mask> <exit-interface>
```

## How I will verify it

After configuring a static route, I will check:

```text
show ip route
show ip route static
show running-config
ping <destination-ip-address>
traceroute <destination-ip-address>
```

## Lab notes

- **Topology:** To be added after the lab.
- **IP addressing:** To be added from the actual lab.
- **Configuration performed:** To be added after I complete the configuration.
- **Verification and observations:** To be added from my router output and test results.
- **Screenshots:** To be added after the lab.

> I have left the lab-specific details blank rather than adding a sample topology or claiming a configuration that I have not recorded yet.
