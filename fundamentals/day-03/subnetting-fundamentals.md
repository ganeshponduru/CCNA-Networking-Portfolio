# Subnetting Fundamentals

## 1. Start with an IPv4 network

I use 192.168.0.0/24 as a simple example. The /24 prefix means the first 24 bits identify the network and the remaining 8 bits are available for host addresses.

- Network address: 192.168.0.0
- First conventional usable host: 192.168.0.1
- Last conventional usable host: 192.168.0.254
- Broadcast address: 192.168.0.255
- Total addresses: 256
- Conventional usable host addresses: 254

The last octet has 8 bits, so there are 2^8 = 256 possible combinations. In a conventional subnet, the all-zero host portion identifies the network and the all-one host portion is the broadcast address.

## 2. Private IPv4 ranges

The RFC 1918 private IPv4 blocks are:

| Private block | Prefix | Address range |
|---|---|---|
| 10.0.0.0–10.255.255.255 | /8 | 10.0.0.0/8 |
| 172.16.0.0–172.31.255.255 | /12 | 172.16.0.0/12 |
| 192.168.0.0–192.168.255.255 | /16 | 192.168.0.0/16 |

The 192.168.0.0/16 private block contains 256 /24-sized networks. A specific /24, such as 192.168.1.0/24, is only one part of that block.

## 3. How I check if a destination is local

A host uses its configured IP address and subnet mask to determine its local network. It applies the mask to its own address and to the destination address, then compares the network results.

Example:

- My PC: 192.168.1.1/24
- Destination: 192.168.20.1
- My local network: 192.168.1.0/24
- Destination network when checked with my /24 mask: 192.168.20.0/24

The network results differ, so the destination is remote from my PC's point of view. My PC checks its routing table and normally sends the packet to a next hop, commonly the default gateway. The sending PC does not learn the destination device's subnet mask from its IP address alone.

## 4. What subnetting means

Subnetting divides a larger address block into smaller networks by extending the network prefix and borrowing bits from the host portion. It helps separate groups, control broadcast domains when combined with Layer 2/Layer 3 design, and allocate address space more appropriately.

If I need at least x equal-sized subnets, I can calculate borrowed bits with:

2^n >= x

Here, n is the number of bits borrowed from the host portion. If I need at least h conventional usable host addresses in each subnet, I need enough host bits b to satisfy:

2^b - 2 >= h

I should not mix up the subnet-count calculation with the host-count calculation.

## 5. Example: /24 to /26

Starting prefix: /24. Borrowing two bits gives /26. The subnet mask becomes 255.255.255.192.

- Borrowed bits: 2
- Remaining host bits: 6
- Number of equal-sized subnets created from the /24: 2^2 = 4
- Total addresses per /26: 2^6 = 64
- Conventional usable hosts per /26: 64 - 2 = 62

The block size in the last octet is 256 - 192 = 64, so the subnet network addresses start at .0, .64, .128, and .192.

## 6. Common prefix and mask values

| Prefix | Subnet mask | Total addresses | Conventional usable hosts |
|---|---|---:|---:|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

These usable-host counts apply to conventional IPv4 subnets; special-purpose prefixes such as /31 point-to-point links use different rules.

## 7. Matching subnet sizes to host requirements

The example groups have 35, 40, and 30 devices. A /26 supports up to 62 conventional usable hosts, so it can fit each group if the device count is the only requirement and the addressing plan allows it.

A /26 is too small for 70 hosts. A /25 provides 126 conventional usable host addresses. For 300 hosts, a /23 provides 510 conventional usable host addresses, though the correct allocation depends on the available address space and wider network design.

## What I need to remember

1. Identify the given prefix and subnet mask.
2. Work out how many subnet bits or host bits are required.
3. Calculate the block size and list the network boundaries.
4. Identify each subnet's network, usable range, and broadcast address.
5. Check whether the question asks for total addresses or usable hosts.
6. Use CIDR rather than relying on classful defaults for modern network planning.

These are worked study notes. I will document practical configuration separately after I complete and verify a lab.
