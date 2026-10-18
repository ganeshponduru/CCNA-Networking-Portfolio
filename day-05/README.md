# Day 05 — ARP, ICMP, Ping, and Inter-Subnet Routing

## Overview

I used VPCS hosts, a switch, a router, and Wireshark to study how ARP and ICMP work in a local subnet and how traffic travels between two IPv4 subnets.

## Case Study 1 — Same-subnet communication

PC1 (`192.168.1.1/24`) pinged PC2 (`192.168.1.2/24`). My capture shows ARP resolution followed by ICMP Echo Requests and Replies.

![Same-subnet topology](./Screenshot%202026-10-18%20232631.png)

![Successful ping and Wireshark packet list](./Screenshot%202026-10-18%20233353.png)

In the [packet-level walkthrough](packet-level-walkthrough.md#case-1--pc1-pings-pc2-on-the-same-subnet), I explain the ARP request, ARP reply, Ethernet addresses, IPv4 fields, and ICMP messages using the expanded packet screenshots.

## Case Study 2 — Routing between different subnets

I configured PC3 (`192.168.1.1/24`) and PC4 (`192.168.20.1/24`) on separate networks connected through R1.

![Different-subnet topology and gateway addresses](./Screenshot%202026-10-18%20235903.png)

My Wireshark capture on the PC3-to-R1 G1/0 link shows ICMP Echo Requests from `192.168.1.1` to `192.168.20.1` and Echo Replies in the reverse direction. A separate capture shows PC3 resolving its default gateway `192.168.1.100` and successfully pinging that gateway.

![ICMP exchange between PC3 and PC4](./Screenshot%202026-10-19%20005453.png)

![ARP resolution and ping to the default gateway](./Screenshot%202026-10-19%20010125.png)

These screenshots support the observations stated above, but they do not show every packet on both router links. The detailed walkthrough distinguishes captured evidence from the expected routing sequence.

## Additional observation — Gratuitous ARP

This separate capture shows repeated broadcast packets labelled as gratuitous ARP for several IPv4 addresses. The screenshot alone does not establish an IP conflict or the reason for each announcement.

[View the gratuitous ARP capture](./Screenshot%202026-10-19%20005413.png)

## What I learned

- ARP resolves the IPv4 address of a local next hop to a MAC address.
- A host normally ARPs for the destination host when it is on the same subnet, but for the default gateway when the destination is remote.
- ICMP Echo Request and Echo Reply messages are used by ping to test IP-layer reachability.
- A router forwards an IP packet using a new Layer 2 frame on the outgoing network; normally, the end-to-end IP source and destination remain unchanged without NAT.
- Gateway ARP is ordinary ARP, not proof that Proxy ARP is in use.
- A successful ping does not prove that a specific application or port is available.

## Detailed notes and revision

- [Full packet-level walkthrough with screenshots](packet-level-walkthrough.md)
- [Different-subnet routing lab](different-subnet-routing-lab.md)
- [ARP fundamentals](arp-fundamentals.md)
- [ICMP and ping](icmp-and-ping.md)
- [Routed ping verification checklist](routing-lab-checklist.md)
- [Practice questions](practice-questions.md)

## Evidence scope

I captured the same-subnet ARP/ICMP exchange, the gateway ARP and ping, and ICMP traffic between PC3 and PC4 on the PC3-to-R1 link. The full packet flow on the R1-to-PC4 link is not shown in the screenshots currently documented here.
