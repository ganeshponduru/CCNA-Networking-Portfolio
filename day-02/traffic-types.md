# Unicast, Broadcast and Multicast

I learned to distinguish these traffic types by asking how many destinations should receive a frame or packet.
| Type | Meaning | Example |
|---|---|---|
| Unicast | One sender to one intended destination | Client connecting to a web server |
| Broadcast | One sender to all applicable devices in a broadcast domain | IPv4 ARP request on a LAN |
| Multicast | One sender to a selected group | IP multicast stream |

## Ethernet forwarding
- Unicast MAC identifies one interface, subject to configuration and forwarding-table state.
- Ethernet broadcast MAC is FF:FF:FF:FF:FF:FF.
- Ethernet multicast uses a group MAC address. IPv4 multicast maps to a range of Ethernet multicast MAC addresses.

A switch learns source MAC addresses and uses its MAC address table to forward known unicast frames toward the appropriate port. Unknown unicast and broadcast frames are generally flooded within the applicable VLAN, excluding the ingress port. Multicast handling depends on configuration and features such as IGMP snooping.

## IPv4 broadcast caveat
IPv4 has limited broadcast address 255.255.255.255 and directed-broadcast concepts. A router does not normally forward Layer 2 Ethernet broadcasts between networks. The claim “a router broadcasts whenever it sees 255s” is inaccurate: forwarding depends on the destination IP, subnet, and configuration; directed broadcasts are generally disabled by default for security.

**Interview answer:** Unicast targets one destination, broadcast targets all relevant devices in a broadcast domain, and multicast targets a group. Ethernet broadcast and IPv4 broadcast are related but distinct Layer 2 and Layer 3 concepts.