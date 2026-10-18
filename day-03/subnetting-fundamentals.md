# Subnetting Fundamentals

## 1. Starting point from the lesson

The instructor first revisits IPv4 address ranges and uses `192.168.0.0` as the example private address block. He then walks through the addresses in a /24 network:

- Network: `192.168.0.0/24`
- First traditionally usable host: `192.168.0.1`
- Last traditionally usable host: `192.168.0.254`
- Broadcast address: `192.168.0.255`

A /24 contains 256 total IPv4 addresses because the last octet has 8 host bits: (2^8 = 256). In a conventional IPv4 subnet, the network address and directed broadcast address are not assigned to ordinary hosts, leaving 254 usable host addresses.

The next /24 network is `192.168.1.0/24`, then `192.168.2.0/24`, and so on. This progression is useful for understanding the instructor's example, but the block `192.168.0.0/16` is the private range; it contains 256 individual /24-sized networks if divided that way.

## 2. Network address and broadcast address

- **Network address / network ID:** identifies the subnet itself. For `192.168.1.0/24`, the network address is `192.168.1.0`.
- **Broadcast address:** the last address in a conventional IPv4 subnet, used to reach all hosts on that subnet. For `192.168.1.0/24`, it is `192.168.1.255`.
- **Usable host addresses:** in a conventional /24, these are `192.168.1.1` through `192.168.1.254`.

These familiar rules have exceptions for special prefix lengths, such as /31 point-to-point links and /32 host routes. For ordinary CCNA subnetting exercises, the network/broadcast/usable-host model is commonly used unless the question says otherwise.

## 3. How a host decides whether a destination is local

The instructor's key question is: how does PC1 know whether a destination IP belongs to its own subnet if it cannot “look” at the topology?

A host uses its configured IP address and subnet mask (or prefix length) to calculate the local network. In the usual IPv4 model:

1. The host performs a bitwise AND between its own IP address and its subnet mask.
2. It performs a bitwise AND between the destination IP address and the **host's own subnet mask**.
3. It compares the two results.

If the results match, the destination is considered on-link, and the host tries to deliver directly on the local Layer 2 network (using ARP to resolve the destination IPv4 address to a MAC address on Ethernet). If the results differ, the host sends the packet to its configured next-hop router/default gateway, assuming a suitable route exists.

### Worked comparison

Assume:
- PC1: `192.168.1.1/24`
- Destination PC2: `192.168.20.1`
- PC1 subnet mask: `255.255.255.0`

PC1's network calculation:

`192.168.1.1 AND 255.255.255.0 = 192.168.1.0`

Destination calculation using PC1's mask:

`192.168.20.1 AND 255.255.255.0 = 192.168.20.0`

The results differ, so PC1 treats PC2 as remote and sends the packet to its default gateway. A router or another Layer 3 path is needed for communication between these subnets. A switch alone does not route between different IPv4 subnets.

**Important:** A ping may still fail for other reasons even when two hosts are in the same subnet, including VLAN separation, incorrect addressing, firewall rules, or a link problem. Being in the same subnet is not a guarantee that ping will succeed.

## 4. What subnetting means

Subnetting divides an existing address block into smaller IP subnets by extending the network prefix—usually by borrowing bits from the host portion. This lets a network administrator allocate address space to separate teams, VLANs, links, or other network segments according to requirements.

A clear interview definition:

> Subnetting is the process of dividing an IP address block into smaller subnets by increasing the network prefix length, so address space can be allocated more appropriately and networks can be organized into separate Layer 3 segments.

The instructor emphasizes efficient address use. That is a major benefit, but subnetting also helps with network segmentation, broadcast-domain design, routing, and administration.

## 5. Formula for the number of subnets

The lesson uses:

`2^n >= x`

Where:
- `n` = number of bits borrowed from the host portion
- `x` = number of subnets required

For three required subnets:
- Borrow 0 bits: `2^0 = 1` subnet — not enough
- Borrow 1 bit: `2^1 = 2` subnets — not enough
- Borrow 2 bits: `2^2 = 4` subnets — enough

Therefore, borrow 2 bits. The number of resulting equal-size subnets is 4, even though the example only needs 3 immediately.

## 6. Formula for addresses per subnet

If `h` host bits remain:

`Total addresses per subnet = 2^h`

For ordinary IPv4 subnets, the usual usable-host calculation is:

`Usable hosts = 2^h - 2`

In the example, the starting prefix is /24, leaving 8 host bits. Borrowing 2 gives /26, leaving 6 host bits:
- Total addresses per /26: `2^6 = 64`
- Traditionally usable host addresses: `64 - 2 = 62`

## 7. Prefix and mask changes

Starting from a /24:
- Borrow 1 bit → /25 → `255.255.255.128`
- Borrow 2 bits → /26 → `255.255.255.192`
- Borrow 3 bits → /27 → `255.255.255.224`
- Borrow 4 bits → /28 → `255.255.255.240`
- Borrow 5 bits → /29 → `255.255.255.248`
- Borrow 6 bits → /30 → `255.255.255.252`

These are prefix lengths, not separate “class” masks. The same CIDR approach works with any suitable starting prefix.

## 8. Planning around host requirements

The instructor's example has departments with 35, 40, and 30 devices. A /26 provides 62 usable host addresses, so it fits each department individually. A fourth /26 subnet can be kept for future use.

If a team later needs 70 hosts, a /26 is too small. The solution is to redesign the allocation with a larger subnet (for example, a /25 has 126 traditionally usable host addresses), while checking the surrounding address plan and avoiding overlaps. It is not automatically necessary to “move to Class B”; modern subnetting uses CIDR and chooses a suitable prefix based on host count and design requirements.

## 9. Classful ranges: context, not modern design rules

The lesson reviews the traditional classful ranges:
- Class A: first octet 1–126 for ordinary unicast networks in the old classful model
- Class B: first octet 128–191
- Class C: first octet 192–223
- 127.0.0.0/8 is loopback; 0.0.0.0 has special meaning; 224.0.0.0/4 is multicast

The transcript's spoken range “Class C is 191 to 230” is a speech/transcription error. The conventional Class C first-octet range is 192–223. In modern networking, use CIDR prefixes such as /24 and /26 instead of relying on class defaults.

## Quick revision

- /24 = 256 total addresses, conventionally 254 usable hosts.
- A network address identifies a subnet; a broadcast address targets all hosts on that subnet.
- A host uses its own IP address and subnet mask to decide whether a destination is on-link.
- Different subnets generally need Layer 3 routing to communicate.
- `2^n >= required subnets` finds how many bits to borrow.
- `2^h` gives total addresses in a subnet; conventionally subtract 2 for usable hosts.
- /26 = 64 total addresses, 62 conventionally usable hosts.
