# EIGRP

EIGRP (Enhanced Interior Gateway Routing Protocol) is an advanced distance-vector routing protocol. It uses DUAL (Diffusing Update Algorithm) to select routes and maintain loop-free paths.

## Main points

- EIGRP uses an Autonomous System (AS) number. Routers that form an EIGRP neighbor relationship must use the same AS number and compatible metric settings.
- EIGRP uses Hello packets to discover and maintain neighbors.
- Update packets carry routing information. EIGRP uses reliable delivery for updates and acknowledges them.
- Query and Reply packets are used when a route needs to be recalculated and an alternate path is required.
- EIGRP uses IP protocol number 88 and multicast address 224.0.0.10 for IPv4 neighbor discovery.
- By default, the EIGRP metric uses bandwidth and delay.

## Configuration used in the lab

The EIGRP AS number was **90**.

R3's visible configuration was:

~~~cisco
router eigrp 90
 network 8.8.8.8 0.0.0.0
 network 192.168.13.0 0.0.0.255
 network 192.168.23.0 0.0.0.255
~~~

R2's visible network statements were:

~~~cisco
router eigrp 90
 network 192.168.1.0
 network 192.168.12.0
 network 192.168.23.0
~~~

These are the statements visible in the configuration screenshot; I have not added commands that were not shown.

## Neighbor table

The `show ip eigrp neighbors` output showed these neighbors:

| Local router | Neighbor address | Local interface |
|---|---|---|
| R2 | 192.168.1.100 | Et3/0 |
| R2 | 192.168.12.1 | Gi1/0 |
| R2 | 192.168.23.3 | Gi2/0 |
| R3 | 192.168.13.1 | Gi1/0 |
| R3 | 192.168.23.2 | Gi2/0 |

## Routing table and ping

The routing-table output showed R1 learning **8.8.8.8/32** through 192.168.13.3. R2 also learned 8.8.8.8/32 through 192.168.23.3.

R1's ping to 8.8.8.8 completed with 5 replies out of 5 (100% success). The destination is R3's Loopback0 in this lab, not a test of access to the public Internet.

## Wireshark packet observations

The capture was taken on the R1–R3 link (192.168.13.0/24). The selected packets showed:

| Packet | Source | Destination | EIGRP opcode | Details |
|---|---|---|---|---|
| 59 | 192.168.13.1 | 224.0.0.10 | Hello (5) | AS 90, EIGRP version 2 |
| 67 | 192.168.13.1 | 224.0.0.10 | Update (1) | Sequence number 2; includes 192.168.12.0/24 and 192.168.1.0/24 |
| 68 | 192.168.13.3 | 192.168.13.1 | Hello-format ACK | Acknowledgment number 2 |

Packet 68 is an acknowledgment carried in an EIGRP Hello-format packet with no route data. It is unicast back to 192.168.13.1. It is not a separate EIGRP opcode named ACK.

## Commands used for verification

~~~cisco
show ip eigrp neighbors
show ip route eigrp
show ip route
ping 8.8.8.8
show running-config | section eigrp
~~~

## What I observed

The neighbor table confirmed EIGRP adjacencies, the routing table showed learned routes, and the ping verified reachability to R3's loopback. Wireshark showed a Hello, an Update carrying route entries, and the acknowledgment for Update sequence 2.
