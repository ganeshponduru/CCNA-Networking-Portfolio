# Day 04 — Finding Network and Broadcast Addresses

I'm using these notes to practise subnet calculations from an IP address and prefix. My goal is to find the network address, broadcast address, usable range, and the next subnet without relying only on a calculator.

## My notes

- [Finding network, broadcast, and usable addresses](address-range-calculations.md)
- [Finding the next subnet and subnetting for multiple networks](next-subnet-and-subnet-design.md)
- [Practice questions with answers](practice-questions.md)
- [Points I clarified while studying](things-i-checked.md)

## What I learned

The network address comes from applying a bitwise AND between the IP address and its subnet mask. The broadcast address is found by setting all host bits of that subnet to 1. For a conventional IPv4 subnet, the first usable address is one greater than the network address and the last usable address is one less than the broadcast address.

When the prefix ends in the middle of an octet, I can find the subnet increment using the relevant mask octet: **256 − mask value**. For example, a /21 has mask 255.255.248.0, so the increment in the third octet is 8.

For a required number of subnets, I borrow enough bits to satisfy **2^n ≥ required subnets**. I then add those borrowed bits to the existing prefix. For host requirements, I separately check that **2^h − 2 ≥ required hosts** for a conventional IPv4 subnet.

## Practice status

These are worked study calculations. I have not labelled them as a completed router or Packet Tracer lab. I'll add lab evidence after I configure and verify the addressing myself.
