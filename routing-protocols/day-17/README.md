# EIGRP, VRRP and HSRP

In this lab, I configured EIGRP between three routers and tested gateway redundancy with VRRP and HSRP. I also used Wireshark to look at the control packets exchanged by these protocols.

The addressing in my practical topology is different from the example addressing in the study material. The notes below use the addresses from my actual lab.

## Topology and addressing

The three routers form a triangle. R1 and R2 connect to the same LAN through Switch1. PC1 uses the virtual gateway, and R3 has Loopback0 configured as the test destination.

| Device | Interface | Address |
|---|---|---|
| R1 | G1/0 | 192.168.12.1/24 |
| R1 | G2/0 | 192.168.13.1/24 |
| R1 | E3/0 | 192.168.1.100/24 |
| R2 | G1/0 | 192.168.12.2/24 |
| R2 | G2/0 | 192.168.23.2/24 |
| R2 | E3/0 | 192.168.1.101/24 |
| R3 | G1/0 | 192.168.23.3/24 |
| R3 | G2/0 | 192.168.13.3/24 |
| R3 | Loopback0 | 8.8.8.8/32 |
| PC1 | LAN | 192.168.1.1/24 |

The shared virtual gateway used in the practical work was **192.168.1.150**. The PDF example used a different LAN subnet and virtual IP, so those example addresses are not used in the practical observations here.

## Notes

| Topic | Notes |
|---|---|
| EIGRP configuration, neighbors and routes | [EIGRP](eigrp.md) |
| VRRP configuration, advertisements and failover | [VRRP](vrrp.md) |
| HSRP configuration, advertisements and failover | [HSRP](hsrp.md) |
| Differences between the two gateway protocols | [VRRP and HSRP comparison](vrrp-and-hsrp-comparison.md) |
| Revision | [Practice questions](practice-questions.md) |

## Practical results

- EIGRP neighbors formed in AS 90.
- R1 learned the route to 8.8.8.8/32 through R3 and pinged it successfully.
- VRRP Group 10 used 192.168.1.150 as its virtual IP. R1 was configured with priority 150 and R2 used the default priority 100.
- After R1 Ethernet3/0 was shut down, R2 became VRRP Master. The PC's first traceroute hop changed from 192.168.1.100 to 192.168.1.101.
- HSRP Group 10 used the same virtual IP after VRRP was removed. R1 was Active and R2 Standby; after R1 Ethernet3/0 was shut down, R2 became Active.
- The continuous ping tests showed a brief timeout during failover, followed by successful replies.

## Screenshots

The screenshot filenames and their intended order are listed in [screenshots](screenshots/README.md). The image files still need to be added to that folder.
