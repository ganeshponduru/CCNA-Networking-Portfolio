# Day 05 — Ping, ARP, and ICMP in a Same-Subnet Lab

## What I worked on

I built a small topology with two VPCS hosts connected to a switch and tested connectivity between them. I used Wireshark to inspect the packets generated when PC1 pinged PC2.


## Screenshots and lab evidence

I uploaded the screenshots from my lab to this folder. The images below show the topology, ping results, and selected Wireshark packet details.

### Case Study 1 — Same subnet

**Topology:** PC1 and PC2 are connected through Switch1 and use addresses in `192.168.1.0/24`.

![Case Study 1 topology](./Screenshot%202026-10-18%20232631.png)

**Ping results and packet list:** The terminal shows five replies from PC2. The Wireshark list shows the ARP exchange followed by ICMP Echo requests and replies.

![Ping results and Wireshark packet list](./Screenshot%202026-10-18%20233353.png)

**ARP request details:** The request asks for `192.168.1.2` and is carried in a broadcast Ethernet frame.

![ARP request details](./Screenshot%202026-10-18%20233444.png)

**IPv4 header:** The selected packet shows source `192.168.1.1`, destination `192.168.1.2`, and protocol `ICMP (1)`.

![IPv4 and ICMP packet details](./Screenshot%202026-10-18%20233707.png)

### Case Study 2 — Different subnets

**Topology:** PC3 and PC4 use different /24 networks and reach each other through R1. The image documents the topology and configured gateway addresses; the routed packet flow is explained separately in the packet-level walkthrough.

![Case Study 2 routed topology](./Screenshot%202026-10-18%20235903.png)

**Additional capture 1 — Gratuitous ARP announcements:** This capture shows repeated broadcast ARP packets labelled as gratuitous ARP for several IPv4 addresses. These packets are distinct from the gateway ARP exchange used to forward traffic to a remote subnet. The screenshot alone does not establish an IP conflict or explain why each announcement occurred.

![Gratuitous ARP announcements](./Screenshot%202026-10-19%20005413.png)

**Additional capture 2 — ICMP across different subnets:** Wireshark is capturing on the PC3-to-R1 G1/0 link. The packet list shows Echo Requests from `192.168.1.1` to `192.168.20.1` and Echo Replies in the reverse direction. This supports successful routed ping traffic on the captured link. The displayed list does not show the complete ARP exchange or every packet on the destination-side router link.

![ICMP traffic on the PC3-to-R1 link](./Screenshot%202026-10-19%20005453.png)

**Additional capture 3 — Gateway ARP and ping:** This capture shows PC3 asking for the MAC address of its default gateway, `192.168.1.100`, and receiving an ARP reply. It then shows ICMP Echo Requests from `192.168.1.1` to `192.168.1.100` and replies from the gateway. This verifies gateway address resolution and reachability; it is a ping to the gateway, not the remote PC4.

![Gateway ARP resolution and ping](./Screenshot%202026-10-19%20010125.png)

## Topology and addressing

| Device | IPv4 address | Subnet mask | MAC address shown in capture |
|---|---|---|---|
| PC1 | 192.168.1.1/24 | 255.255.255.0 | 00:50:79:66:68:00 |
| PC2 | 192.168.1.2/24 | 255.255.255.0 | 00:50:79:66:68:01 |

Both PCs are in the same IPv4 subnet, so PC1 can send traffic directly to PC2 at Layer 2. A default gateway is not needed for this same-subnet test.

## What I observed

- The ping from PC1 to `192.168.1.2` received five replies.
- In the capture, I saw an ARP request asking who has `192.168.1.2`, followed by an ARP reply.
- After ARP resolution, Wireshark showed ICMP Echo requests from `192.168.1.1` to `192.168.1.2` and Echo replies in the opposite direction.
- The IPv4 header showed protocol number `1`, which identifies ICMP.
- The Ethernet destination for the ARP request was the broadcast MAC `ff:ff:ff:ff:ff:ff`. The ICMP frames were sent to the destination host's MAC address.

## Packet flow I learned

1. PC1 checks whether it already knows the MAC address for `192.168.1.2`.
2. If the mapping is missing, PC1 broadcasts an ARP request on the local LAN.
3. PC2 recognises its own IPv4 address in the ARP target and sends an ARP reply containing its MAC address.
4. PC1 can then send the ICMP Echo request in an Ethernet frame addressed to PC2.
5. PC2 sends an ICMP Echo reply back to PC1.

The exact capture can differ if the ARP mapping is already cached. In that case, the ping may begin with ICMP packets and no new ARP exchange.

## Evidence

- The topology screenshot shows PC1 and PC2 connected through Switch1.
- The terminal screenshot shows the configured PC1 address and five successful ping replies.
- The Wireshark packet list shows the ARP request/reply followed by ICMP Echo requests and replies.
- The expanded ARP packet shows sender IP `192.168.1.1`, target IP `192.168.1.2`, and broadcast target MAC.
- The expanded IPv4 header shows source `192.168.1.1`, destination `192.168.1.2`, and `ICMP (1)`.

## Commands used

```text
PC1> ip 192.168.1.1/24
PC1> ping 192.168.1.2
```

I captured traffic on the PC1-to-switch link in Wireshark.

## Key takeaways

- Ping is a utility that uses ICMP Echo messages to test IP-layer reachability and measure round-trip time.
- ARP resolves an IPv4 address to a link-layer MAC address on the local Ethernet network.
- An ARP request is broadcast; a normal ARP reply is usually unicast to the requester.
- A switch learns source MAC addresses from incoming Ethernet frames and uses its MAC address table to forward known unicast traffic.
- IP addresses identify the Layer 3 endpoints, while MAC addresses deliver a frame across the local Layer 2 link.

## Scope

This README includes same-subnet ARP/ICMP evidence, the different-subnet topology, a capture of ICMP requests and replies on the PC3-to-R1 link, and a separate capture showing gratuitous ARP announcements. The captures show gateway ARP resolution and a successful ping to the gateway, as well as ICMP requests and replies between PC3 and PC4 visible on the PC3-to-R1 link. They do not verify every ARP step or the full packet flow on both router links.

## Packet-level walkthrough

- [Full packet-level walkthrough: ARP, ICMP, and routing](packet-level-walkthrough.md)

## Case Study 2 — Routing between different subnets

- [Different-subnet routing lab](different-subnet-routing-lab.md)
- [Verification checklist for routed ping](routing-lab-checklist.md)
