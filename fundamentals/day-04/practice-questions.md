# Practice Questions — Subnet Calculations

I use these questions to check whether I can work out the answer instead of memorising a range.

## Questions

1. For 10.25.79.90/22, find the subnet mask, network address, broadcast address, usable range, and number of usable hosts.
2. For 102.224.96.32/20, find the network address, broadcast address, and usable range.
3. What is the next network after 192.168.10.0/24?
4. For 190.168.10.50/21, find the current network, broadcast address, and next network.
5. How many bits must be borrowed to create at least 7 equal-sized subnets from a /24? What is the resulting prefix?
6. How many equal-sized subnets can be created by borrowing 3 bits?
7. What prefix is needed to create at least 50 equal-sized subnets from 10.0.0.0/8?
8. Can a /23 support 300 conventional usable IPv4 host addresses?

## Answer key

1. Mask 255.255.252.0; network 10.25.76.0/22; broadcast 10.25.79.255; usable 10.25.76.1–10.25.79.254; 1,022 usable hosts.
2. Network 102.224.96.0/20; broadcast 102.224.111.255; usable 102.224.96.1–102.224.111.254.
3. 192.168.11.0/24.
4. Network 190.168.8.0/21; broadcast 190.168.15.255; next network 190.168.16.0/21.
5. Borrow 3 bits because 2^3 = 8; new prefix /27.
6. 2^3 = 8 equal-sized subnets.
7. Borrow 6 bits because 2^6 = 64; new prefix /14, mask 255.252.0.0.
8. Yes. A /23 has 512 total addresses and 510 conventional usable host addresses.

I should try each question before checking the answer key.
