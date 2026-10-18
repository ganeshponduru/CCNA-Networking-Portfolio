# Day 03 — Subnetting Review and Interview Questions

## Questions from the lesson

### 1. What is subnetting?
Subnetting divides an IP address block into smaller subnets by extending the network prefix and borrowing bits from the host portion. It helps allocate addresses efficiently and organize network segments.

### 2. Why did the example borrow two bits?
The company needed at least three subnets. `2^n >= x`; with x = 3, borrowing two bits gives `2^2 = 4` subnets.

### 3. What mask results from borrowing two bits from /24?
The prefix becomes /26, which is `255.255.255.192`.

### 4. How many total and usable addresses are in a /26?
There are 6 host bits, so `2^6 = 64` total addresses. In a conventional IPv4 subnet, 62 are usable host addresses after excluding the network and broadcast addresses.

### 5. What are the four /26 networks inside 192.168.1.0/24?
- `192.168.1.0/26`
- `192.168.1.64/26`
- `192.168.1.128/26`
- `192.168.1.192/26`

### 6. How does a PC know whether a destination is in its own subnet?
It uses its configured IP address and prefix/subnet mask to calculate its local network, then checks the destination using that prefix. If on-link, it attempts direct Layer 2 delivery; if off-link, it sends the packet to a next-hop router.

### 7. Can two hosts in different IP subnets communicate through a Layer 2 switch alone?
Not through ordinary Layer 2 switching alone. Communication between IP subnets normally requires a router or Layer 3 switch and suitable routes.

### 8. Does a /26 always support 62 devices?
It has 62 traditionally usable host addresses, but some may be needed by a default gateway or other interfaces. Check the actual host requirement and design.

### 9. A department needs 70 host addresses. Is /26 enough?
No. A /26 has 62 traditionally usable hosts. A /25 has 126 usable hosts, so it may fit if the surrounding address plan permits it.

### 10. A subnet needs 300 hosts. Must the company “move to Class B”?
No. Modern networks use CIDR. A /23 provides 510 traditionally usable host addresses and may fit 300 hosts, subject to design and address availability.

## Quick calculations

| Prefix | Subnet mask | Total addresses | Conventional usable hosts |
|---|---|---:|---:|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

*The conventional usable-host formula does not apply unchanged to special cases such as /31 point-to-point links.*

## Practice before the next class

Try these without looking at the answer first:

1. How many subnets result from borrowing 3 bits from a /24?
2. What prefix and mask result from borrowing 3 bits from /24?
3. How many total addresses and conventional usable hosts are in each subnet?
4. What is the block size for `255.255.255.224`?
5. List the network and broadcast addresses for the first four /27 subnets inside `192.168.1.0/24`.
6. A team needs 50 host addresses. Is /26 sufficient? Explain.
7. PC-A is `10.1.1.10/24`; PC-B is `10.1.2.10/24`. Are they in the same subnet? What does PC-A normally do to reach PC-B?

## Answers for self-check

1. `2^3 = 8` subnets.
2. /27, `255.255.255.224`.
3. 32 total addresses, 30 conventional usable hosts.
4. `256 - 224 = 32`.
5. Networks: `192.168.1.0/27`, `.32/27`, `.64/27`, `.96/27`; broadcasts: `.31`, `.63`, `.95`, `.127`.
6. Yes, /26 has 62 conventional usable hosts, leaving 12 usable host slots before considering gateways or infrastructure.
7. No. Their /24 network IDs are `10.1.1.0` and `10.1.2.0`; PC-A normally sends traffic to its configured gateway, assuming a route exists.
