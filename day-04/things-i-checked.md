# Points I Clarified While Studying

I wrote down these checks because they help me avoid common subnetting mistakes.

- **Use the prefix that is actually given.** Historical Class A/B/C ranges are background; a /20 or /21 prefix determines the subnet calculation.
- **Network address and broadcast address are not normal host addresses** in a conventional IPv4 subnet.
- **Host bits and borrowed bits are different.** Borrowed bits become part of the network prefix; the bits left over determine the size of each subnet.
- **For subnet count**, I use 2^n ≥ required subnets. For conventional usable host count, I use 2^h − 2 ≥ required hosts.
- **The increment is based on the changing mask octet.** For example, a /21 mask is 255.255.248.0, so the third-octet increment is 256 − 248 = 8.
- **The next network is not the same as the broadcast address.** The broadcast address of the current subnet is one less than the next subnet's network address.
- **A /14 has 18 host bits, not 2.** In the second octet, 2 bits remain as host bits, but the entire third and fourth octets are host bits too.
- **A /23 has 512 total addresses and 510 conventional usable host addresses.** It can support 300 hosts in address-count terms, though actual design must also account for gateways, infrastructure, growth, and the full addressing plan.
- **A subnet calculator is a cross-check, not a replacement for understanding.** I want to be able to explain how I got the network ID and range.
