# Static Routing

## What is static routing?

Static routing is the process of adding routes to a router's routing table manually. The administrator specifies the destination network, its subnet mask, and the next hop (or an outgoing interface).

A router uses its routing table to decide where to forward a packet. If it has no matching route and no usable default route, it drops the packet.

## Static route command

```text
Router(config)# ip route <destination-network> <subnet-mask> <next-hop-ip>
```

Example:

```text
Router(config)# ip route 192.168.20.0 255.255.255.0 192.168.1.2
```

- **Destination network:** the remote network to reach.
- **Subnet mask:** identifies the destination network's prefix.
- **Next-hop IP:** the neighbouring router to which the packet should be sent.

A static route can also be configured using an exit interface. The next-hop form is used in the examples above.

## Administrative distance

Administrative distance (AD) indicates the preference given to a route source when a router has competing routes to the same destination prefix. A lower AD is preferred.

| Route source | Default AD |
|---|---:|
| Directly connected | 0 |
| Static | 1 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| RIP | 120 |

A static route can be configured with a different AD. In the Day 10 dual-ISP example, the primary and backup routes are assigned different AD values so the preferred path is selected first.

## Route selection

The key order discussed in class is:

1. **Longest prefix match:** the most specific matching route is selected.
2. **Lowest administrative distance:** used when competing routes have the same prefix length.
3. **Metric:** used by a routing protocol to compare paths when applicable.

Do not compare AD before prefix length. A more-specific route can be selected even if its route source has a higher AD.

## Advantages

- The administrator controls the configured path.
- Static routes do not require routers to exchange routing updates.
- They are useful in small or stable networks.
- They can be used to provide a backup path with a higher AD.

## Limitations

- Routes must be configured and maintained manually.
- A router does not automatically learn topology changes through static routing.
- Large networks with many destination networks become difficult to maintain.
- A backup static route only becomes useful when the preferred route is removed from the routing table and the backup route is otherwise reachable.

## Verification commands

```text
show ip route
show ip interface brief
ping <destination-ip>
traceroute <destination-ip>
```

- `show ip route` displays the routing table.
- `show ip interface brief` helps check interface status and IP addressing.
- `ping` tests reachability to a destination.
- `traceroute` shows the Layer 3 hops seen along the path.

## Day 10 lab connection

The Day 10 lab used static routes within the simulated provider-side topology to provide reachability between the office network and the simulated destination. The PC's successful ping and the path shown by traceroute are documented in the [Day 10 lab overview](README.md).

The handout contains example addressing and configuration values. The lab observations in this repository use the addresses visible in the captured lab outputs; the two should not be mixed when describing the topology.
