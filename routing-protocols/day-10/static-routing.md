# Static Routing

## Meaning

A static route is a route that an administrator manually adds to a router's routing table. The route tells the router which next hop to use to reach a destination network.

## Command format

```text
ip route <destination-network> <subnet-mask> <next-hop-address>
```

Example format:

```text
ip route 192.168.20.0 255.255.255.0 192.168.1.2
```

The destination network and mask identify the remote network. The next-hop address identifies the neighbouring router to which the packet should be forwarded.

## How a router uses the route

When a packet arrives, the router checks its routing table and selects the most specific matching route. If a matching route is found, the router forwards the packet towards the next hop or outgoing interface. If no specific route matches, the router may use a default route if one is configured.

## Administrative distance

Administrative distance (AD) is used to compare route sources for the same destination prefix. A lower AD is preferred. For static routes, the default administrative distance is commonly 1; an administrator can configure a different value.

## Advantages

- The administrator controls the path.
- Static routes do not require routing-protocol updates.
- They can be useful in small or stable networks and as backup routes.

## Limitations

- Routes must be maintained manually.
- Changes in network topology are not automatically learned through static routing.
- Large networks can become difficult to maintain using only static routes.

## Verification

```text
show ip route
ping <destination-ip>
traceroute <destination-ip>
```

The Day 10 lab's router routing-table output is available in the [day overview](README.md#lab-screenshots).
