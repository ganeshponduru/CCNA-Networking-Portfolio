# Worked Example — Split 192.168.1.0/24 into Four /26 Subnets

I worked through this example to see how a /24 can be divided into four equal /26 subnets and how the ranges line up.

## Requirement

Three departments need separate subnets:
- Sales: 35 devices
- Technical: 40 devices
- HR: 30 devices

Starting network: `192.168.1.0/24`. A fourth subnet can be reserved for future use.

## Step 1: Find borrowed bits

The company needs at least 3 subnets. Use `2^n >= x`, where n is borrowed bits and x is required subnets.

- `2^1 = 2`: not enough
- `2^2 = 4`: enough

Borrow 2 bits from the 8 host bits in /24. The new prefix is /26.

## Step 2: Calculate the mask and subnet size

- /26 mask: `255.255.255.192`
- Host bits left: 6
- Total addresses per subnet: `2^6 = 64`
- Conventional usable host addresses: `64 - 2 = 62`
- Block size in the last octet: `256 - 192 = 64`

Start at 0 and add 64 each time.

## Step 3: Subnet ranges

| Subnet | Network address | Usable host range | Broadcast address |
|---|---|---|---|
| 1 | `192.168.1.0/26` | `192.168.1.1–192.168.1.62` | `192.168.1.63` |
| 2 | `192.168.1.64/26` | `192.168.1.65–192.168.1.126` | `192.168.1.127` |
| 3 | `192.168.1.128/26` | `192.168.1.129–192.168.1.190` | `192.168.1.191` |
| 4 | `192.168.1.192/26` | `192.168.1.193–192.168.1.254` | `192.168.1.255` |

## Step 4: Allocate departments

| Team | Example subnet | Usable hosts available | Devices needed | Usable host slots left |
|---|---|---:|---:|---:|
| Sales | `192.168.1.0/26` | 62 | 35 | 27 |
| Technical | `192.168.1.64/26` | 62 | 40 | 22 |
| HR | `192.168.1.128/26` | 62 | 30 | 32 |
| Future use | `192.168.1.192/26` | 62 | — | — |

Each department fits in a /26. The fourth subnet remains available if it is not allocated elsewhere.

## Step 5: Check the arithmetic

The original /24 contains 256 total addresses. Four /26 subnets contain 4 × 64 = 256 total addresses. Subnetting has not created extra addresses; it has divided the same block into smaller subnets.

Three separate /24 allocations reserve 768 total addresses. Three /26 allocations reserve 192 total addresses for these departments, a difference of 576 addresses in allocated block size. The fourth /26 is still part of the original /24 and can be reserved for future use. Real designs also account for gateways, infrastructure, and expected growth.

## Important notes

- Do not assign the network or broadcast address to an ordinary host in these conventional /26 subnets.
- A default gateway can use one of the usable addresses, such as `192.168.1.1` in the first subnet, if that address is selected and configured on the router or Layer 3 interface. It is not automatically always .1.
- Hosts in different /26 subnets need Layer 3 routing to communicate.
- A subnet mask identifies the network/host bits; it does not by itself guarantee connectivity.
