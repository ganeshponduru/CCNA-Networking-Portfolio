# VRRP and HSRP comparison

Both protocols provide first-hop redundancy by allowing hosts to use a virtual default gateway. Their terminology and control-packet details are different.

| Feature | VRRP | HSRP |
|---|---|---|
| Router roles | Master / Backup | Active / Standby |
| Virtual IP in this lab | 192.168.1.150 | 192.168.1.150 |
| Group number in this lab | 10 | 10 |
| R1 priority | 150 | 150 |
| R2 priority | 100 (default) | 100 (default) |
| IPv4 control traffic | IP protocol 112 | UDP port 1985 |
| IPv4 multicast destination shown | 224.0.0.18 | 224.0.0.2 |
| State after R1 LAN interface shutdown | R2 became Master | R2 became Active |

## What was common in my tests

- PC1 used the virtual IP 192.168.1.150 as its default gateway.
- R1 was the preferred router because it had priority 150.
- R2 used priority 100.
- When R1's Ethernet3/0 interface was shut down, R2 took over.
- The continuous ping showed a brief timeout during each transition.
- Traceroute showed the first hop changing from R1's physical address (192.168.1.100) to R2's physical address (192.168.1.101).

## Main difference

VRRP uses Master and Backup states, while HSRP uses Active and Standby states. The Wireshark captures also show different protocol handling: VRRP uses IP protocol 112, while the HSRP packets in this capture use UDP port 1985.

These are the behaviors observed in this lab; the table is not intended to cover every version or optional feature of either protocol.
