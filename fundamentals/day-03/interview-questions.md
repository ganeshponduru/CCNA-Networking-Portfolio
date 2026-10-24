# Day 03 — Subnetting Revision Questions

Use these questions to check whether I can explain subnetting without simply memorising the examples.

## 1. What is subnetting?

Subnetting divides an IP address block into smaller networks by extending the network prefix and borrowing bits from the host portion. It helps organize networks and allocate addresses according to requirements.

## 2. Why does borrowing two bits create four subnets?

Each borrowed bit has two possible values. Two borrowed bits create 2^2 = 4 combinations, so a /24 divided into equal /26 subnets produces four subnets.

## 3. What mask belongs to /26?

The mask is 255.255.255.192. Six host bits remain in the last octet, giving 64 total addresses and 62 conventional usable host addresses per subnet.

## 4. What are the four /26 networks inside 192.168.1.0/24?

- 192.168.1.0/26, broadcast 192.168.1.63
- 192.168.1.64/26, broadcast 192.168.1.127
- 192.168.1.128/26, broadcast 192.168.1.191
- 192.168.1.192/26, broadcast 192.168.1.255

## 5. Can a /26 support 70 hosts?

No. It has 62 conventional usable host addresses. A /25 has 126 conventional usable host addresses, assuming an appropriately aligned block is available.

## 6. How do I find the number of borrowed bits?

For at least x equal-sized subnets, use 2^n >= x, where n is the number of borrowed bits.

## 7. How do I find the number of host bits?

For at least h conventional usable host addresses, use 2^b - 2 >= h, where b is the number of host bits.

## 8. What does a host do when the destination is on another subnet?

It checks its routing table and normally forwards the packet to a next hop, often its default gateway. It resolves the next-hop link-layer address as needed; it does not send the remote host's MAC address directly across the router path.

## 9. Is the default gateway always the .1 address?

No. .1 is a common convention, but the gateway can use another valid address in the subnet. The configured gateway must match the actual network design.

## 10. Why is CIDR important?

CIDR uses explicit prefixes such as /24 and /26 rather than relying on historical classful defaults. It supports more flexible address allocation and route aggregation.

## Self-check

Before I call a subnetting problem finished, I should be able to show the prefix, mask, block size, network address, usable host range, broadcast address, and the calculation behind the result.
