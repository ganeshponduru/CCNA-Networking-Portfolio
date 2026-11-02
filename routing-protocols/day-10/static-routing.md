# Day 10 — Static Routing

## What I learned

Static routing is the process of adding routes manually to a router's routing table. The router forwards packets using the destination network and the configured next hop.

The Cisco IOS command format is:

```text
ip route <destination-network> <subnet-mask> <next-hop-ip-address>
```

A static route can also use an exit interface. The appropriate form depends on the topology and interface type.

## Lab topology

The lab uses five routers:

- **R1:** Main LAN router and DHCP server
- **R3:** Airtel path
- **R4:** Tata path
- **R5:** Upstream router representing Google, with loopback address `8.8.8.8/32`
- **R2:** Connected to the LAN switch; its routing role is not shown in the captured routing tables

PC1 and PC2 connect to Switch1. The switch connects to R1 and R2.

### IP addressing

| Link/network | Device | Interface IP |
|---|---|---|
| LAN `192.168.1.0/24` | R1 | `192.168.1.100` |
| `192.168.13.0/24` | R1 | `192.168.13.1` |
| `192.168.13.0/24` | R3 (Airtel) | `192.168.13.3` |
| `192.168.14.0/24` | R1 | `192.168.14.1` |
| `192.168.14.0/24` | R4 (Tata) | `192.168.14.4` |
| `192.168.35.0/24` | R3 | `192.168.35.3` |
| `192.168.35.0/24` | R5 | `192.168.35.5` |
| `192.168.45.0/24` | R4 | `192.168.45.4` |
| `192.168.45.0/24` | R5 | `192.168.45.5` |
| Loopback | R5 | `8.8.8.8/32` |

## Routes visible in the lab

### R3 — Airtel

The captured routing table shows:

- `8.8.8.8/32` via `192.168.35.5` (R5)
- `192.168.1.0/24` via `192.168.13.1` (R1)
- Connected networks: `192.168.13.0/24` and `192.168.35.0/24`

### R4 — Tata

The captured routing table shows:

- `8.8.8.8/32` via `192.168.45.5` (R5)
- `192.168.1.0/24` via `192.168.14.1` (R1)
- Connected networks: `192.168.14.0/24` and `192.168.45.0/24`

### R5 — Google

The final captured routing table shows:

- `192.168.13.0/24` via `192.168.35.3`, AD 10
- `192.168.14.0/24` via `192.168.45.4`, AD 20
- `192.168.1.0/24` via both `192.168.45.4` and `192.168.35.3`, each with AD 1
- Connected networks: `192.168.35.0/24`, `192.168.45.0/24`, and loopback `8.8.8.8/32`

The two installed routes to `192.168.1.0/24` have the same administrative distance and metric in the displayed table. This is equal-cost routing, not a primary/backup pair.

The R5 terminal screenshot also visibly shows this command being entered:

```text
ip route 192.168.1.0 255.255.255.0 192.168.45.4
```

## Verification and observations

From PC1:

- Ping to R1's `192.168.13.1` succeeded.
- Ping to R3's `192.168.35.3` succeeded.
- The first ping attempt to `8.8.8.8` timed out, while the following attempt received five replies.
- Traceroute to `8.8.8.8` showed the path through `192.168.1.100`, `192.168.13.3`, and `192.168.35.5`.
- The final traceroute response was ICMP type 3, code 3 (destination port unreachable). The ping test to `8.8.8.8` succeeded, so this traceroute response should not be described as proof that end-to-end IP reachability failed.

From R5:

- Traceroute to `192.168.1.1` showed responses involving both the R3 and R4 paths before reaching the LAN.
- Ping to `192.168.1.1` reported 4 successful replies out of 5 (80% success).

The R5 ping result shows reachability, but 20% packet loss means this test was not completely clean. It should not be presented as a zero-loss result.

## Commands used for verification

```text
show ip route
ping <destination-ip-address>
traceroute <destination-ip-address>
```

## Screenshots

The router routing-table outputs and PC1/R5 connectivity tests are saved in this Day 10 folder. They show the configured routes and the results described above.
