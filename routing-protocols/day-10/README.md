# Day 10 — Static Routing, Default Routing, and ISP Failover

## What I learned in this class

In this class, I learned how routers use static routes and a default route to send traffic towards another network. We also discussed administrative distance and how a floating static route can be used as a backup when the primary route is no longer available.

The instructor explained the example using two ISPs:
- Airtel as the primary connection.
- Tata as the backup connection.

The idea was to configure both paths so that traffic normally uses Airtel, but can move to Tata if the Airtel path goes down.

## What I did in the lab

I worked on a topology with an office LAN, two ISP routers (Airtel and Tata), and a router representing the Google destination. The goal of the lab was to give the PCs connectivity to the destination address `8.8.8.8`, then test whether the backup ISP path works when the primary path is shut down.

![Day 10 lab topology](./day-10-topology.png)

### IP addresses shown in my topology

| Device / interface | IP address |
|---|---|
| PC1 | 192.168.1.1/24 |
| R1 office LAN (e3/0) | 192.168.1.100/24 |
| R1 to Airtel (e3/3) | 192.168.13.1/24 |
| R3 Airtel to R5 | 192.168.35.3/24 |
| R5 Airtel-side interface | 192.168.35.5/24 |
| R1 to Tata (e3/4) | 192.168.14.1/24 |
| R4 Tata to R5 | 192.168.45.4/24 |
| R5 Tata-side interface | 192.168.45.5/24 |
| R5 Loopback0 | 8.8.8.8/32 |

PC1 received these settings in the displayed `show ip` output:
- IP address: `192.168.1.1/24`
- Default gateway: `192.168.1.100`
- DNS: `8.8.8.1`
- DHCP server: `192.168.1.100`

## Static route and default route

A static route is entered manually to tell a router how to reach a destination network. The general command format discussed in class is:

```text
ip route <destination-network> <subnet-mask> <next-hop>
```

A default route is used when the routing table does not have a more specific route for the destination:

```text
ip route 0.0.0.0 0.0.0.0 <next-hop>
```

In this lab, R1 had two default routes. The route table showed Airtel as the active route:

```text
S* 0.0.0.0/0 [10/0] via 192.168.13.3
```

The configured preference was:
- Airtel next hop: `192.168.13.3`, administrative distance 10.
- Tata next hop: `192.168.14.4`, administrative distance 20.

Because both routes are default routes for the same prefix, the route with the lower administrative distance is preferred. The higher-AD static route is called a floating static route; it can be used when the preferred route is withdrawn.

## What I observed during testing

### 1. PC1 reached 8.8.8.8

I checked PC1's IP settings and then ran:

```text
ping 8.8.8.8
```

The screenshot shows successful replies from `8.8.8.8`. R1's own ping test also shows 5 successful replies out of 5.

![PC1 IP settings and successful ping through the primary path](./day-10-primary-ping.png)

### 2. Traffic initially used Airtel

I ran traceroute from PC1:

```text
trace 8.8.8.8
```

The hops shown were:
1. `192.168.1.100` — R1, the office gateway.
2. `192.168.13.3` — Airtel.
3. `192.168.35.5` — R5, the simulated Google destination side.

This showed that the initial path was through Airtel.

### 3. I shut down the Airtel-facing interface

On R1, I entered interface configuration mode for Ethernet3/3 and used `shutdown`. The console displayed the interface and line protocol changing to down.

I also tested a ping from R1 after shutting the interface. The screenshot shows that R1 could still ping `8.8.8.8` successfully.

![R1 Ethernet3/3 shutdown and interface state messages](./day-10-airtel-shutdown.png)

### 4. Traffic moved to Tata

After the Airtel path went down, I ran traceroute again from PC1. The hops changed to:
1. `192.168.1.100` — R1.
2. `192.168.14.4` — Tata.
3. `192.168.45.5` — R5, the simulated Google destination side.

This showed that traffic was now using the Tata path instead of Airtel.

![PC1 traceroute showing the Tata backup path](./day-10-tata-failover.png)

I also ran a continuous ping from PC1. The screenshot shows successful replies continuing during the test after the path change.

![Continuous ping from PC1](./day-10-continuous-ping.png)

## What I understood from this lab

- A static route is manually configured on a router.
- A default route is used when there is no more-specific matching route.
- Administrative distance helps select between routes to the same destination prefix.
- A floating static route can act as a backup route.
- Ping helped me check whether the destination was reachable.
- Traceroute helped me see which ISP path was being used.
- Shutting down the Airtel-facing interface allowed me to test whether traffic could use the Tata backup path.

## A note about the lab result

In this topology, `8.8.8.8` is configured as a loopback address on R5. So this lab demonstrates successful connectivity to a simulated Internet destination inside the lab, not a connection to the real public Internet. The screenshots provided here do not show a NAT configuration.

## Commands I used during verification

```text
show ip
show ip route
ping 8.8.8.8
trace 8.8.8.8
config terminal
interface e3/3
shutdown
no shutdown
```

## Class material

- Topic: Static Routing and Default Routing
- Instructor: E. Ramesh Goud
- Training: TungaBadra Networks
