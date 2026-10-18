# Finding the Next Subnet and Planning Subnets

## Finding the next subnet

I first identify the subnet increment. When the prefix ends inside an octet, I use:

**Increment = 256 − the subnet-mask value in the changing octet**

Then I add that increment to the relevant octet to find the next network address.

### Example: 192.168.10.25/24

- Mask: 255.255.255.0
- The prefix ends on an octet boundary, so the network is 192.168.10.0/24.
- The next /24 network is 192.168.11.0/24.
- The current subnet's broadcast address is 192.168.10.255.

### Example: 190.168.10.50/21

I use the /21 prefix, rather than relying on the address's historical class.

- Mask: 255.255.248.0
- The changing octet is the third octet.
- Increment: 256 − 248 = 8.
- The value 10 falls in the 8–15 block.

| Item | Result |
|---|---|
| Current network | 190.168.8.0/21 |
| First usable address | 190.168.8.1 |
| Last usable address | 190.168.15.254 |
| Broadcast address | 190.168.15.255 |
| Next network | 190.168.16.0/21 |

A useful cross-check: the broadcast address is one less than the next network address.

## Planning for a required number of subnets

To create at least a required number of equal-sized subnets, I use:

**2^n ≥ number of required subnets**

Here, n is the number of host bits borrowed for subnetting. The new prefix is the original prefix plus n.

### Example: divide 192.168.0.0/24 into at least 7 subnets

- Required subnets: 7
- 2^2 = 4, which is not enough.
- 2^3 = 8, so I need to borrow 3 bits.
- New prefix: /24 + 3 = /27.
- Host bits remaining: 5.
- Each /27 contains 2^5 = 32 total addresses, or 30 conventional usable host addresses.

The eight subnet network addresses are:

| Subnet | Network address | Broadcast address | Usable host range |
|---|---|---|---|
| 1 | 192.168.0.0/27 | 192.168.0.31 | 192.168.0.1–192.168.0.30 |
| 2 | 192.168.0.32/27 | 192.168.0.63 | 192.168.0.33–192.168.0.62 |
| 3 | 192.168.0.64/27 | 192.168.0.95 | 192.168.0.65–192.168.0.94 |
| 4 | 192.168.0.96/27 | 192.168.0.127 | 192.168.0.97–192.168.0.126 |
| 5 | 192.168.0.128/27 | 192.168.0.159 | 192.168.0.129–192.168.0.158 |
| 6 | 192.168.0.160/27 | 192.168.0.191 | 192.168.0.161–192.168.0.190 |
| 7 | 192.168.0.192/27 | 192.168.0.223 | 192.168.0.193–192.168.0.222 |
| 8 | 192.168.0.224/27 | 192.168.0.255 | 192.168.0.225–192.168.0.254 |

I need at least seven, so eight equal-sized subnets satisfy the requirement.

### Example: divide 10.0.0.0/8 into at least 50 subnets

- 2^5 = 32, which is not enough.
- 2^6 = 64, so I borrow 6 bits.
- New prefix: /8 + 6 = /14.
- Mask: 255.252.0.0.
- Each /14 has 18 host bits, so it contains 262,144 total addresses and 262,142 conventional usable host addresses.
- The increment is 256 − 252 = 4 in the second octet.

The first few network addresses are 10.0.0.0/14, 10.4.0.0/14, 10.8.0.0/14, 10.12.0.0/14, and 10.16.0.0/14. This gives 64 equal-sized subnets, enough for a requirement of 50.

## Planning for host requirements

The number of required subnets and the number of required hosts are different calculations.

For conventional IPv4 subnets, I check **2^h − 2 ≥ required hosts**, where h is the number of host bits.

For example, a requirement of 300 hosts can fit in a /23:

- Total addresses: 2^9 = 512
- Conventional usable host addresses: 512 − 2 = 510
- Mask: 255.255.254.0

If I allocate 172.16.0.0/23, the range is 172.16.0.0–172.16.1.255, with usable host addresses 172.16.0.1–172.16.1.254. That leaves capacity for growth, but the correct allocation still depends on the wider addressing plan and any other network requirements.

Modern subnet design uses CIDR prefixes. I should not assume I must switch to a historical address class just because I need more hosts.
