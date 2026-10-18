# Case Study 2 — Ping Between Different Subnets

## What I configured

I extended my VPCS lab by connecting PC3 and PC4 to two interfaces on router R1. I configured the router interfaces with gateway addresses and set the default gateway on both VPCS hosts. After that, I was able to ping PC4 from PC3.

## Topology and IP addressing

| Device/interface | IPv4 address | Subnet mask | Purpose |
|---|---|---|---|
| PC3 | 192.168.1.1 | 255.255.255.0 (/24) | Source host |
| R1 G1/0 | 192.168.1.100 | 255.255.255.0 (/24) | PC3's default gateway |
| R1 G2/0 | 192.168.20.200 | 255.255.255.0 (/24) | PC4's default gateway |
| PC4 | 192.168.20.1 | 255.255.255.0 (/24) | Destination host |

The two IPv4 networks are:

- PC3 LAN: `192.168.1.0/24`
- PC4 LAN: `192.168.20.0/24`

## Packet flow I learned

1. PC3 compares the destination IP address `192.168.20.1` with its local subnet and determines that the destination is remote.
2. PC3 sends the traffic toward its configured default gateway, `192.168.1.100`.
3. If the gateway MAC address is not already in PC3's ARP cache, PC3 broadcasts an ARP request for `192.168.1.100`. It does not ARP directly for PC4's MAC address because PC4 is on another subnet.
4. PC3 sends an Ethernet frame addressed to R1's G1/0 MAC address. The IP destination remains `192.168.20.1`.
5. R1 examines the IP destination and forwards the packet out G2/0, because `192.168.20.0/24` is directly connected to that interface.
6. If R1 does not already know PC4's MAC address, it uses ARP on the destination LAN to resolve `192.168.20.1`.
7. R1 sends a new Ethernet frame toward PC4. The router changes the Layer 2 frame for the next link; under normal routing without NAT, the source and destination IP addresses remain the same, apart from normal IP header changes such as TTL decrement.
8. PC4 sends the reply back through its default gateway, `192.168.20.200`, and R1 routes it toward PC3.

## What I verified

- I configured the router interfaces G1/0 and G2/0 with `192.168.1.100` and `192.168.20.200`, respectively.
- I configured the default gateways on the VPCS hosts.
- I was then able to ping PC4 from PC3.

My Wireshark screenshot captured on the PC3-to-R1 G1/0 link shows ICMP Echo Requests from `192.168.1.1` to `192.168.20.1` and Echo Replies from `192.168.20.1` to `192.168.1.1`. This is packet-list evidence that the ping exchange crossed the subnet boundary and that the return traffic was visible on this link. The screenshot does not show the complete gateway ARP exchange or the R1-to-PC4 link, so I do not claim those individual steps were captured.

## Proxy ARP clarification

I initially labelled the case study with “Proxy ARP.” The behavior required for this topology is ordinary routing through a configured default gateway, with ARP used to resolve the gateway's MAC address and, when needed, the destination host's MAC on the remote LAN.

That gateway ARP exchange is not itself Proxy ARP. Proxy ARP is a separate router behavior in which the router responds to an ARP request on behalf of another IP address under specific conditions.

## Key takeaway

For a remote IPv4 destination, a host sends the frame to its next hop (normally the default gateway), while the IP packet is addressed to the remote host. A router routes the IP packet and uses a new Layer 2 frame on each Ethernet network it forwards across.
