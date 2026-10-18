# Subnetting — Things I Corrected or Clarified

While reviewing my subnetting notes, I found a few statements and transcript fragments that needed checking. I recorded the corrected concepts here so I can revise them later. Some wording may have come from speech-to-text errors, so I don't treat unclear fragments as definite quotes.

## 1. Class C range

The traditional Class C range is first-octet values **192–223**, not 191–230. Traditional classful ranges are historical: Class A commonly used 1–126 for ordinary unicast networks, Class B 128–191, and Class C 192–223. The 127/8 block is reserved for loopback, and 224–239 is used for multicast.

Today, subnetting is based on CIDR prefixes such as /24 and /26, not on choosing a class.

## 2. Private range 192.168.0.0/16

The private block 192.168.0.0/16 runs from 192.168.0.0 to 192.168.255.255. It contains 256 separate /24 blocks. It is not one single /24 network. For example, 192.168.1.0/24 is one of those blocks.

## 3. Total addresses vs usable host addresses

For a conventional IPv4 /24 subnet:

- Total addresses: 256
- Network address: 192.168.1.0
- Usable host range: 192.168.1.1 to 192.168.1.254
- Broadcast address: 192.168.1.255
- Conventional usable host addresses: 254

The first and last addresses are not assigned to ordinary hosts in a conventional subnet. Special-purpose prefixes such as /31 have different rules for point-to-point links.

## 4. How a host decides whether a destination is local

Suppose my PC is 192.168.1.1/24 and the destination is 192.168.20.1. My PC uses its own subnet mask to calculate the network portion of its own address and the destination address. The results differ, so the destination is not on the local /24 subnet. The PC checks its routing table and normally sends the packet to a next hop, often its default gateway.

The sending host does not know the destination host's subnet mask just from the destination IP address.

## 5. Choosing the number of subnets

If I need at least x equal-sized subnets, I can find the number of borrowed bits n using 2^n >= x.

For ordinary IPv4 subnets, if I need at least h usable host addresses, I need enough host bits b to satisfy 2^b - 2 >= h. These are different calculations: one is for the number of subnets, and the other is for usable hosts per subnet.

## 6. Splitting 192.168.1.0/24 into four /26 subnets

Borrowing two bits from the host portion changes /24 to /26. The mask is 255.255.255.192. Each /26 has 64 total addresses and 62 conventional usable host addresses.

| Subnet | Usable host range | Broadcast |
|---|---|---|
| 192.168.1.0/26 | 192.168.1.1–192.168.1.62 | 192.168.1.63 |
| 192.168.1.64/26 | 192.168.1.65–192.168.1.126 | 192.168.1.127 |
| 192.168.1.128/26 | 192.168.1.129–192.168.1.190 | 192.168.1.191 |
| 192.168.1.192/26 | 192.168.1.193–192.168.1.254 | 192.168.1.255 |

## 7. How much address space is saved?

The answer depends on what I compare.

- Three /24 blocks contain 768 total addresses.
- Three /26 blocks contain 192 total addresses: 576 fewer total addresses allocated.
- If I reserve all four /26 subnets from one /24, the four subnets still total 256 addresses. They divide the original block; they do not create extra addresses.
- Three /24 blocks have 762 conventional usable host slots; three /26 blocks have 186, a difference of 576 usable slots.

I should state which allocation I am comparing instead of saying a subnetting plan simply saves a number without explaining the basis.

## 8. A /26 is not enough for 70 hosts

A /26 has 62 conventional usable host addresses, so it cannot fit 70 hosts in one subnet. A /25 has 128 total addresses and 126 conventional usable host addresses, if an appropriately aligned block is available.

A requirement for 300 hosts does not mean I should automatically move to Class B. With CIDR, a /23 has 512 total addresses and 510 conventional usable host addresses. The right allocation still depends on the available address block and network design.

## 9. Default gateway

A default gateway is the next-hop router used when a destination has no more-specific matching route. Its address is not always .1; that is just a common convention. The gateway address must be valid for the local subnet and match the actual network configuration.

## 10. Broadcast traffic

A Layer 2 broadcast is delivered within its broadcast domain, such as a VLAN. Routers do not normally forward ordinary Layer 2 broadcasts between networks. IPv4 also has limited and directed broadcast concepts; directed broadcasts are commonly disabled on routers for security reasons.

## 11. Reserved addresses

An organization may reserve a range for static devices or exclude it from DHCP, but there is no universal rule that .1 through .10 must be reserved. I need to follow the actual addressing plan for each network.

## Revision reminder

Before finalizing a subnetting answer, I should identify the prefix, calculate the block size, list the network and broadcast addresses, and then check the usable host range. I should also confirm whether the question asks for total addresses, usable hosts, or the number of subnets.
