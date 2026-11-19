# HSRP

HSRP (Hot Standby Router Protocol) is a first-hop redundancy protocol. Routers share a virtual IP address for the hosts' default gateway. The Active router forwards traffic and the Standby router is ready to take over if the Active router becomes unavailable.

## Configuration used in the lab

HSRP was configured on the LAN-facing Ethernet3/0 interfaces using group 10 and virtual IP 192.168.1.150.

**R1**

~~~cisco
interface e3/0
 standby 10 ip 192.168.1.150
 standby 10 priority 150
~~~

**R2**

~~~cisco
interface e3/0
 standby 10 ip 192.168.1.150
~~~

R1 used priority 150 and R2 used the default priority 100. The `show standby brief` output showed R1 as Active and R2 as Standby before the failure test.

## Failover test

R1's Ethernet3/0 interface was shut down. The router output showed R1's HSRP state moving to Init and R2 changing from Standby to Active.

PC1 continued pinging 8.8.8.8. One timeout is visible during the transition, followed by successful replies.

The traceroute after failover showed:

| Hop | Address | Device |
|---|---|---|
| 1 | 192.168.1.101 | R2 |
| 2 | 192.168.23.3 | R3 |

This confirms that the first hop moved to R2 after it became Active. The second hop returned ICMP Type 3, Code 3 (Destination Port Unreachable) from the simulated destination path.

## Wireshark packet observations

The HSRP capture showed packets from 192.168.1.100 to multicast address **224.0.0.2**.

The selected Advertisement packet showed:

| Field | Value |
|---|---|
| Source IP | 192.168.1.100 |
| Destination IP | 224.0.0.2 |
| TTL | 1 |
| IP protocol | UDP (17) |
| UDP source and destination ports | 1985 |
| HSRP version | 0 |
| Opcode | Advertise (3) |
| Advertised state | Active (3) |

The packet list also showed Hello messages in Speak, Standby and Active states. This capture records HSRP control messages and state information.

## Verification commands

~~~cisco
show standby brief
show standby
show ip eigrp neighbors
~~~

The lab demonstrated Active/Standby election, takeover by R2, and the corresponding HSRP messages in Wireshark.
