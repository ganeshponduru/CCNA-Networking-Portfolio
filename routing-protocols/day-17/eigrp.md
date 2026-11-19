# EIGRP

EIGRP is a dynamic routing protocol. It uses DUAL (Diffusing Update Algorithm) to choose routes and keep the routing information loop-free.

## How EIGRP works

- Routers use Hello packets to find and maintain neighbors.
- Update packets carry routing information to neighbors.
- Acknowledgments confirm that reliable EIGRP updates have been received.
- Query and Reply packets are used when EIGRP needs to find another path to a destination.
- EIGRP for IPv4 uses IP protocol 88 and multicast address 224.0.0.10.
- The default metric is based on bandwidth and delay.

## Configuration

I used EIGRP AS 90 in this lab.

R3:

~~~cisco
router eigrp 90
 network 8.8.8.8 0.0.0.0
 network 192.168.13.0 0.0.0.255
 network 192.168.23.0 0.0.0.255
~~~

R2:

~~~cisco
router eigrp 90
 network 192.168.1.0
 network 192.168.12.0
 network 192.168.23.0
~~~

## EIGRP neighbors

The routers formed these neighbor relationships:

| Router | Neighbor address | Interface |
|---|---|---|
| R2 | 192.168.1.100 | Et3/0 |
| R2 | 192.168.12.1 | Gi1/0 |
| R2 | 192.168.23.3 | Gi2/0 |
| R3 | 192.168.13.1 | Gi1/0 |
| R3 | 192.168.23.2 | Gi2/0 |

## Routes and connectivity

R1 learned the route to **8.8.8.8/32** through 192.168.13.3. R2 also learned the same route through 192.168.23.3.

I pinged 8.8.8.8 from R1 and received all 5 replies. In this topology, 8.8.8.8 is R3's Loopback0 address.

## EIGRP packets in Wireshark

I captured EIGRP packets on the R1–R3 link (192.168.13.0/24).

| Packet | Source | Destination | Packet type | Details |
|---|---|---|---|---|
| 59 | 192.168.13.1 | 224.0.0.10 | Hello | EIGRP AS 90, version 2 |
| 67 | 192.168.13.1 | 224.0.0.10 | Update | Sequence number 2; carries 192.168.12.0/24 and 192.168.1.0/24 |
| 68 | 192.168.13.3 | 192.168.13.1 | Acknowledgment | Acknowledges Update sequence number 2 |

The acknowledgment is carried in an EIGRP Hello-format packet. It is sent directly back to 192.168.13.1; ACK is not a separate EIGRP opcode.

## Commands

~~~cisco
show ip eigrp neighbors
show ip route eigrp
show ip route
ping 8.8.8.8
show running-config | section eigrp
~~~
