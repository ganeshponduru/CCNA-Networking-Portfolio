# Day 03 — IPv4 Subnetting

I used these notes to make subnetting easier to understand step by step: first identifying a network and its usable addresses, then checking whether two IP addresses belong to the same subnet, and finally dividing a larger network into smaller subnets.

## My notes

- [Subnetting fundamentals and formulas](subnetting-fundamentals.md)
- [Worked example: splitting 192.168.1.0/24 into four /26 subnets](worked-example.md)
- [Things I corrected or clarified while studying](correction-log.md)
- [Revision and interview questions](interview-questions.md)

## What I learned

A /24 contains 256 total IPv4 addresses. In a conventional subnet, the first address identifies the network and the last is the broadcast address, leaving 254 usable host addresses.

Borrowing two host bits from a /24 gives a /26. That creates four equal-sized subnets, each containing 64 total addresses and 62 conventionally usable host addresses. I worked through the ranges in the worked example.

A host compares the network portion of its own IP address and the destination IP address using its subnet mask. If the destination is on the local subnet, it can send directly on the local link. If it is remote, the host normally sends the packet to a next hop, commonly its default gateway.

## Important distinction

Classful addressing is historical background. For modern network planning, I need to understand CIDR prefixes and choose a subnet based on the required number of hosts and the overall addressing plan.

## Practice status

The calculations here are written study examples. I have not claimed to complete a router, switch, or Packet Tracer lab. I'll add practical evidence when I perform the configuration and verify the results.
