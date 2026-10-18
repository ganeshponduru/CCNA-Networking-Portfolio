# ARP Fundamentals

## What I learned

Address Resolution Protocol (ARP) is used on IPv4 Ethernet networks to find the MAC address associated with a local IPv4 next hop. ARP messages are carried directly in Ethernet frames; they are not carried inside an IPv4 packet.

## ARP request and reply

- **ARP request:** asks which device owns a target IPv4 address. It is normally sent to the Ethernet broadcast address `ff:ff:ff:ff:ff:ff`.
- **ARP reply:** tells the requester the sender's MAC address for the requested IPv4 address. A normal reply is generally unicast to the requester.
- **ARP cache:** stores recently learned IPv4-to-MAC mappings so a host does not need to broadcast for every packet.

In my same-subnet lab, PC1 (`192.168.1.1/24`) needed the MAC address for PC2 (`192.168.1.2/24`). The capture showed the ARP exchange before the ICMP traffic.

## How the switch fits in

A Layer 2 switch learns a source MAC address on the port where a frame arrives. It floods broadcast frames within the VLAN, then forwards known unicast frames based on its MAC address table. The switch does not perform the host's IPv4-to-MAC resolution; the endpoints use ARP.

## Same subnet versus remote subnet

If the destination is on the same subnet, a host normally ARPs for the destination host's MAC address.

If the destination is on a different subnet, the host normally ARPs for the MAC address of its configured default gateway. The IP packet's destination remains the remote host's IP address, while the Ethernet frame is addressed to the gateway's local interface MAC address.

Resolving the default gateway's MAC is ordinary ARP behavior. It is not, by itself, Proxy ARP. Proxy ARP is a separate behavior in which a router answers an ARP request on behalf of another IP address under certain conditions.

## Gratuitous ARP and RARP

- **Gratuitous ARP (GARP):** an ARP announcement or probe in which the sender and target protocol addresses are the same. It can help detect duplicate IPv4 addresses or announce a changed IP-to-MAC mapping. Exact behavior depends on the implementation and message form.
- **RARP:** Reverse ARP is a historical mechanism for discovering an IP address from a MAC address. It is obsolete in modern networks and was superseded by mechanisms such as BOOTP and DHCP.

## Troubleshooting notes

An absent ARP entry does not automatically prove a physical-layer fault. I would also check the IP address and subnet mask, VLAN, interface/link state, gateway configuration where relevant, and whether ARP traffic is being filtered or answered by another device. A successful ping is useful evidence of IP-layer reachability at that moment, but it does not guarantee that every application or service is working.
