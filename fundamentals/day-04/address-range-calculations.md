# Finding Network, Broadcast, and Usable Addresses

I learned to solve these questions by checking the prefix first, finding the subnet mask, and then calculating the network address. After that, the broadcast and usable range are easier to find.

## Method I use

1. Write down the IP address and prefix.
2. Convert the prefix into a subnet mask.
3. Apply a bitwise AND between the IP address and mask to find the network address.
4. Set all host bits to 1 to find the broadcast address.
5. For a conventional IPv4 subnet, add 1 to the network address for the first usable address and subtract 1 from the broadcast address for the last usable address.
6. Calculate the number of addresses from the host-bit count: total addresses = 2^(host bits); conventional usable hosts = total addresses − 2.

If an octet in the mask is 255, the corresponding IP octet stays unchanged during the AND operation. If the mask octet is 0, the network-address octet becomes 0. The octet between those values is where I usually need to do the binary calculation.

## Example 1: 10.25.79.90/22

A /22 means 22 network bits and 10 host bits.

- Subnet mask: 255.255.252.0
- The changing octet is the third octet.
- The mask value 252 gives an increment of 256 − 252 = 4.
- Third-octet subnet ranges therefore begin at 0, 4, 8, 12, …, 72, 76, 80, …
- The value 79 falls in the 76–79 block.

| Item | Result |
|---|---|
| Network address | 10.25.76.0/22 |
| First usable address | 10.25.76.1 |
| Last usable address | 10.25.79.254 |
| Broadcast address | 10.25.79.255 |
| Total addresses | 1,024 |
| Conventional usable host addresses | 1,022 |

There are 10 host bits, so 2^10 = 1,024 total addresses. In a conventional IPv4 subnet, 1,022 are usable by hosts.

## Example 2: 102.224.96.32/20

A /20 means 20 network bits and 12 host bits.

- Subnet mask: 255.255.240.0
- The changing octet is the third octet.
- The mask value 240 gives an increment of 256 − 240 = 16.
- The third-octet blocks begin at 0, 16, 32, 48, 64, 80, 96, 112, and so on.
- The value 96 is the start of its block, so the network address is 102.224.96.0.

| Item | Result |
|---|---|
| Network address | 102.224.96.0/20 |
| First usable address | 102.224.96.1 |
| Last usable address | 102.224.111.254 |
| Broadcast address | 102.224.111.255 |
| Total addresses | 4,096 |
| Conventional usable host addresses | 4,094 |

The host-bit count is 12, so 2^12 = 4,096 total addresses.

## Quick checks

- The network address has all host bits set to 0.
- The broadcast address has all host bits set to 1.
- The first and last usable addresses are between those two addresses.
- I should calculate using the given prefix, not assume the prefix from the historical address class.
