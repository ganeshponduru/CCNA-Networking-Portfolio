# Day 10 — Static Routing, Default Routing and ISP Redundancy

## Objective

Provide PC1 in the office LAN with connectivity to the simulated Internet destination 8.8.8.8 through two ISP paths. Airtel is the primary path and Tata is the backup. The final test shuts down the Airtel-facing interface and checks whether traffic moves to Tata.

**Lab result:** PC1 successfully pinged 8.8.8.8. Before failover, traceroute used 192.168.13.3 (Airtel). After the Airtel link was shut down, traceroute used 192.168.14.4 (Tata), and continuous ping replies continued.

## 1. Topology and IP addressing

The topology contains the office LAN, R1 (bank/DHCP router), R3 (Airtel), R4 (Tata), and R5 (Google simulation).

| Device / link | Interface | Address |
|---|---|---|
| PC1 | e0 | 192.168.1.1/24 |
| R1 — office LAN | e3/0 | 192.168.1.100/24 |
| R1 — Airtel/R3 | e3/3 | 192.168.13.1/24 |
| R3 — R5 | g1/0 | R3: 192.168.35.3/24 |
| R5 — Airtel side | g1/0 | 192.168.35.5/24 |
| R1 — Tata/R4 | e3/4 | 192.168.14.1/24 |
| R4 — R5 | g2/0 | R4: 192.168.45.4/24 |
| R5 — Tata side | g2/0 | 192.168.45.5/24 |
| R5 | Loopback0 | 8.8.8.8/32 |

PC1's displayed settings:
- IP/mask: 192.168.1.1/24
- Gateway: 192.168.1.100
- DNS: 8.8.8.1
- DHCP server: 192.168.1.100

The point-to-point networks are 192.168.13.0/24 (R1–R3), 192.168.14.0/24 (R1–R4), 192.168.35.0/24 (R3–R5), and 192.168.45.0/24 (R4–R5).

## 2. Concepts

### Physical and logical interfaces

A physical interface connects to another device or network. If it is administratively down, enable it with `no shutdown`. A loopback is a logical (virtual) interface created on the router. R5 uses Loopback0 with 8.8.8.8/32 to represent the Google destination. A /32 mask represents one host address.

Example:
```text
interface loopback0
 ip address 8.8.8.8 255.255.255.255
```

### Routing table

A router checks its routing table to decide where to forward packets based on the destination IP. The command used in the lab is:

```text
show ip route
```

Route codes visible in the output:
- C — directly connected network
- L — local interface address
- S — static route
- S* — candidate default route

### Static route

A static route is manually added by an administrator. It identifies the destination network or host, subnet mask, and next-hop IP.

```text
ip route <destination-network> <subnet-mask> <next-hop-ip>
```

Example host route to the simulated Google loopback:

```text
ip route 8.8.8.8 255.255.255.255 <next-hop-ip>
```

The next-hop address must be reachable through a connected network. Static routes are simple and do not require a dynamic routing protocol, but they need manual configuration and maintenance.

### Administrative distance (AD)

AD is used to compare the preference of routes to the same prefix learned from different sources. Lower AD is preferred when the prefix length is the same.

| Route source | Typical AD discussed in class/PDF |
|---|---:|
| Static | 1 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| RIP | 120 |
| iBGP | 200 |
| eBGP | 20 |
| IS-IS | 15 |

The lab explicitly assigns AD 10 and AD 20 to two static default routes. These configured values are used to choose the primary and backup paths.

### Route selection

The router first uses **longest prefix match** (the most specific matching route). If the competing routes have the same prefix length, it compares AD; lower AD is preferred. Metric/cost is then relevant when comparing routes from the same routing source under its selection rules.

For example, a matching /32 is more specific than a /24, even if the /24 has a lower AD. AD is not the first selection rule when prefix lengths differ.

### Default route and floating static route

A default route is used when no more-specific route matches the destination. It forwards traffic to an upstream next hop.

```text
ip route 0.0.0.0 0.0.0.0 <next-hop-ip>
```

With an explicit AD:

```text
ip route 0.0.0.0 0.0.0.0 <next-hop-ip> <AD>
```

R1 uses two default routes:
- Airtel: next hop 192.168.13.3, AD 10 (preferred)
- Tata: next hop 192.168.14.4, AD 20 (backup)

Both are /0 routes, so the lower AD route is preferred while it is available. The higher-AD route is a **floating static route**. It becomes active when the preferred route is withdrawn.

## 3. Lab configuration and verification

### Step 1 — Configure the simulated destination

R5 uses Loopback0 as 8.8.8.8/32. Its physical links connect to R3/Airtel over 192.168.35.0/24 and R4/Tata over 192.168.45.0/24.

### Step 2 — Configure links and check interfaces

Configure the interface addresses shown in the topology and enable the physical interfaces. The interface-state command used is:

```text
show ip interface brief
```

### Step 3 — Ensure the ISP-side routers have return and destination routes

The route-table screenshots/transcript show these next-hop relationships:
- R3/Airtel reaches 8.8.8.8 through 192.168.35.5 and reaches the office LAN through R1 at 192.168.13.1.
- R4/Tata reaches 8.8.8.8 through 192.168.45.5 and reaches the office LAN through R1 at 192.168.14.1.
- R5 has paths back to 192.168.1.0/24 through both ISP routers. The displayed R5 route table prefers Tata with AD 10 and keeps Airtel with AD 20 for that office prefix.

The screenshots show the route-table entries, but do not expose every configuration command for every ISP interface. Missing command lines are therefore not reconstructed as if they were captured.

### Step 4 — Configure R1 default routes

R1 is the office gateway at 192.168.1.100. The ISP-facing addresses are 192.168.13.1 and 192.168.14.1. The class demonstrates:

```text
ip route 0.0.0.0 0.0.0.0 192.168.13.3 10
ip route 0.0.0.0 0.0.0.0 192.168.14.4 20
```

The R1 route-table screenshot shows the active default route:

```text
S* 0.0.0.0/0 [10/0] via 192.168.13.3
```

This confirms that the Airtel route is active with AD 10.

### Step 5 — Verify PC1 and ping

PC1's `show ip` output showed 192.168.1.1/24, gateway 192.168.1.100, and DHCP server 192.168.1.100. PC1 then pinged 8.8.8.8 and received replies. R1's own ping test also showed 100% success (5/5).

### Step 6 — Trace the primary path

Before failover, PC1's traceroute showed:
1. 192.168.1.100 — R1, office gateway
2. 192.168.13.3 — Airtel next hop
3. 192.168.35.5 — R5, Google simulation side

The last traceroute probe reports ICMP type 3, code 3 (destination port unreachable). This is the response shown for the traceroute probe; it does not cancel out the separate successful ping replies.

### Step 7 — Shut down Airtel and verify failover

A continuous ping was started from PC1. The Airtel-facing R1 interface Ethernet3/3 (192.168.13.1) was shut down. The console showed the interface and line protocol going down.

After the preferred route was withdrawn, the Tata floating static default route became active. The subsequent traceroute showed:
1. 192.168.1.100 — R1
2. 192.168.14.4 — Tata next hop
3. 192.168.45.5 — R5, Google simulation side

The continuous-ping capture shows replies continuing after the interface shutdown. The class demonstration reported a brief loss of one packet during the switchover, followed by continued replies. The Airtel interface was later brought back up with `no shutdown`.

## 4. Commands used for verification

| Command | Purpose |
|---|---|
| `show ip interface brief` | Check interface addresses and state |
| `show ip route` | Check connected, local, static and default routes |
| `ping 8.8.8.8` | Test reachability |
| `trace 8.8.8.8` | Identify the path taken |
| `shutdown` | Administratively disable an interface |
| `no shutdown` | Re-enable an interface |

## 5. What I observed

1. PC1 received its IP settings from R1 through DHCP.
2. R1 selected Airtel as the primary default route (AD 10).
3. PC1 successfully pinged the simulated destination 8.8.8.8.
4. The initial traceroute used 192.168.13.3 (Airtel).
5. After Ethernet3/3 was shut down, traceroute used 192.168.14.4 (Tata).
6. Continuous ping replies continued after the failover, demonstrating backup-path operation.

## 6. Accuracy notes and lab limitations

- **This is simulated Internet reachability, not proof of access to the real public Internet.** The address 8.8.8.8 is configured on R5's loopback in this lab.
- The topology uses private addresses on the provider links. The instructor explains that real provider-facing links normally use public IP addresses and that NAT translates private internal addresses for Internet access. NAT configuration is not shown as part of this Day 10 lab.
- The PDF's sample interface/address plan differs from the topology and terminal screenshots supplied for this lab. This README follows the submitted topology and observed 13.x, 14.x, 35.x and 45.x addresses.
- The PDF lists the default static-route AD as 1 in general. Here, explicit AD values 10 and 20 are configured to make Airtel primary and Tata backup.
- The traceroute's destination-port-unreachable response is not evidence that the ping failed. The ping output separately shows successful replies.

## 7. Key takeaways

- Routers use routing tables to forward packets.
- Static routes are manually configured.
- A default route forwards traffic for destinations without a more-specific route.
- Longest prefix match is considered before AD.
- A floating static route can provide a backup path.
- Ping checks reachability; traceroute helps identify the path.
- Dual-ISP designs need valid routing in both directions.

## Source material

- Class PDF: **Static and Default Routing**, TungaBadra Networks.
- Class transcript: static/default routing, administrative distance, longest-prefix match, dual-ISP setup and failover.
- Submitted lab screenshots: topology, route tables, PC1 addressing, ping/traceroute and interface shutdown.

Instructor: E. Ramesh Goud  
Training: TungaBadra Networks
