# Day 05 — Revision Questions

1. **What does ARP do?**  
   It resolves an IPv4 address to a MAC address for communication on the local Ethernet network.

2. **Why is an ARP request normally broadcast?**  
   The sender does not yet know which MAC address owns the target IPv4 address.

3. **What did PC1 do before sending ICMP traffic in my capture?**  
   It sent an ARP request for `192.168.1.2` and received an ARP reply.

4. **What is the ICMP protocol number in IPv4?**  
   `1`.

5. **What are the ICMP Type and Code for Echo Request and Echo Reply?**  
   Echo Request is Type 8, Code 0. Echo Reply is Type 0, Code 0.

6. **If a host pings an IP address in another subnet, whose MAC address does it normally need first?**  
   The MAC address of its configured default gateway, assuming it has no suitable cached mapping.

7. **Does the destination IP change when a router forwards a normal packet toward another network?**  
   Normally, the source and destination IP addresses remain the same end-to-end, while each routed Ethernet hop uses a new Layer 2 frame. Other features such as NAT can change IP addresses.

8. **Is ARP used to discover a remote host's MAC address across routers?**  
   No. The sender resolves the local next hop, normally its default gateway.

9. **Does a missing ARP entry prove a cable or physical interface is faulty?**  
   No. Addressing, VLAN, interface state, filtering, and other causes must also be checked.

10. **Does a successful ping prove an application is reachable?**  
    No. Ping tests ICMP reachability, not whether a specific application or TCP/UDP port is accepting connections.
