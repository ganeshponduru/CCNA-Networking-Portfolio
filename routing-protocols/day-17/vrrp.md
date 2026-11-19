# VRRP

VRRP (Virtual Router Redundancy Protocol) provides first-hop redundancy. Routers in the same group share a virtual IP address that hosts use as their default gateway. One router forwards traffic as Master while another waits as Backup.

## Configuration used in the lab

The VRRP configuration was applied on the LAN-facing Ethernet3/0 interfaces.

**R1**

~~~cisco
interface e3/0
 vrrp 10 ip 192.168.1.150
 vrrp 10 priority 150
~~~

**R2**

~~~cisco
interface e3/0
 vrrp 10 ip 192.168.1.150
~~~

R1's physical LAN address was 192.168.1.100/24. R2's was 192.168.1.101/24. The virtual IP was 192.168.1.150. R1 used priority 150; R2 used the default priority 100.

PC1 was configured as 192.168.1.1/24 with 192.168.1.150 as its gateway.

## Master and Backup

The `show vrrp brief` outputs showed R1 becoming Master after its priority was changed to 150, while R2 became Backup. The group number was 10 and the virtual IP was 192.168.1.150.

## Wireshark observation

The VRRP capture showed advertisements sent to multicast address **224.0.0.18** using IP protocol **112**. The selected packet showed:

| Field | Value |
|---|---|
| Source IP | 192.168.1.100 |
| Destination IP | 224.0.0.18 |
| TTL | 255 |
| VRRP version | 2 |
| Packet type | Advertisement (1) |
| Virtual Router ID | 10 |
| Priority in selected packet | 100 |
| Advertisement interval | 1 second |
| Virtual IP | 192.168.1.150 |

There is a priority difference worth preserving: the later CLI output shows R1 configured with priority 150, but the selected Wireshark packet shows priority 100. The capture may be from the earlier configuration state, but the screenshot alone does not prove when it was taken. I have kept the packet value and CLI value separate rather than treating them as one simultaneous state.

The packet list also shows advertisements first sourced from 192.168.1.100 and later from 192.168.1.101.

## Failover test

A continuous ping was running from PC1 to 8.8.8.8. R1's LAN interface was then shut down:

~~~cisco
interface e3/0
 shutdown
~~~

The router logs showed R1 changing from Master to Init and R2 changing from Backup to Master. The continuous ping had a timeout during the transition and then continued receiving replies.

The traceroute first hop changed:

| Test | First hop | Second hop |
|---|---|---|
| Before R1 interface shutdown | 192.168.1.100 (R1) | 192.168.13.3 (R3) |
| After R1 interface shutdown | 192.168.1.101 (R2) | 192.168.23.3 (R3) |

The second hop returned ICMP Type 3, Code 3 (Destination Port Unreachable) for the simulated destination test. The first-hop change and router state logs are the evidence that R2 took over the gateway.

## Verification commands

~~~cisco
show vrrp brief
show vrrp
show ip eigrp neighbors
~~~

The lab demonstrated Master/Backup election and gateway failover. The ping timeout shows that the transition was not completely loss-free.
