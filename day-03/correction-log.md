# Correction Log — Subnetting Lesson

This log is deliberately separate from the lesson notes so I can see what was said in class, what needs correction, and why. A few transcript fragments are clearly affected by speech-to-text errors; those are marked as transcript issues rather than attributed as certain instructor statements.

## 1. Class C range

**Transcript around 0:19:** “Class is 191 to 230.”

**Issue:** This range is incorrect and appears to be a speech-to-text error. The conventional classful Class C range is 192–223 in the first octet.

**Correct version:** Class A traditionally used first-octet values 1–126 for ordinary unicast networks, Class B 128–191, and Class C 192–223. The 127/8 block is loopback; 224–239 is multicast.

**Modern context:** Classful addressing is historical. Modern subnetting uses CIDR prefixes, such as /24 and /26.

## 2. “Class C private IP range”

**Transcript around 0:34:** The private range is described as `192.168.0.0` to `192.168.255.255`.

**Clarification:** This is the private IPv4 block `192.168.0.0/16`, not a single /24 network. It contains 65,536 total addresses and can be divided into 256 /24-sized blocks, from `192.168.0.0/24` through `192.168.255.0/24`.

**Correct version:** The private block is `192.168.0.0/16`. If using a /24 subnet, `192.168.0.0/24` has network address `.0`, usable host range `.1–.254`, and broadcast `.255`.

## 3. Number of networks in the private block

**Transcript around 1:17–1:34:** The discussion mixes “256 networks” with “2 to the power of 24.”

**Issue:** Those counts refer to different things. A /16 block divided into /24 subnets has 8 bits available for the subnet number, so it contains `2^8 = 256` /24 subnets. The number `2^24` would not be the count of /24 subnets in a /16 block.

**Correct version:** `192.168.0.0/16` contains 256 /24-sized subnets, each with 256 total addresses.

## 4. Total addresses vs usable host addresses

**Transcript around 2:22–5:56:** The instructor counts 0 through 255 as 256 addresses, then identifies .0 and .255 as network and broadcast addresses.

**Status:** The core explanation is correct for an ordinary /24. The wording “available IP addresses” can be confusing because it sometimes means total addresses and sometimes usable host addresses.

**Correct version:**
- 256 total addresses in a /24.
- 254 conventionally usable host addresses.
- Network address: first address.
- Broadcast address: last address.

Use “total addresses” or “usable host addresses” explicitly.

## 5. Same subnet does not guarantee successful ping

**Transcript around 8:13–9:05:** Two hosts in `192.168.0.0/24`, such as `192.168.0.1/24` and `192.168.0.200/24`, are expected to communicate through the same Layer 2 network.

**Clarification:** That is the expected addressing/routing behavior, but ping is not guaranteed. The hosts also need working links, compatible VLAN/L2 connectivity, correct configuration, and no rule blocking ICMP.

**Correct version:** If both hosts are in the same subnet and the Layer 2 path and host settings are correct, they can communicate directly without a router forwarding between subnets.

## 6. How the host determines whether a destination is local

**Transcript around 14:14–19:35:** The host performs a bitwise AND between its source IP and subnet mask, then between the destination IP and the source host's subnet mask. It compares the results.

**Status:** The central idea is correct for the standard IPv4 on-link decision.

**Clarification:** The host uses its configured local prefix/mask to determine whether the destination is on-link. If on-link, it attempts direct Layer 2 delivery; if off-link, it sends the packet to a next-hop router according to its routing table. In real systems, routing tables and interface configuration determine the final decision, so this is a teaching model of the usual case.

**Example:** For `192.168.1.1/24` reaching `192.168.20.1`, the local network is `192.168.1.0/24` and the destination's network under that /24 mask is `192.168.20.0/24`. They differ, so PC1 treats the destination as remote.

## 7. “Network ID” and the subnet mask

**Transcript around 20:32–21:22 and 44:01–45:34:** The network ID is described as representing the host IP address.

**Clarification:** More precisely, the network address identifies the subnet to which a host address belongs. A subnet mask/prefix determines which bits are interpreted as the network prefix and which bits are host bits.

**Correct version:** An IP address plus its prefix identifies the host's subnet. The network address is the result of applying the subnet mask to the IP address.

## 8. Address efficiency example

**Transcript around 22:12–26:42:** Three departments with 35, 40, and 30 devices are assigned separate /24 networks, then subnetting is proposed to reduce unused address space.

**Status:** The example's point is sound. Three separate /24s provide 254 usable host addresses each, which is far more than each department's device count.

**Correction / nuance:** A /26 provides 62 traditionally usable host addresses and fits each team. Three /26 subnets use 192 total addresses within the original /24, and the fourth /26 can be reserved. The exact “wasted” count depends on whether we count usable host slots, all addresses in allocated blocks, and reserved infrastructure addresses. State the metric rather than saying only “650+ IPs wasted.”

## 9. Subnetting definition

**Transcript around 26:16–26:51 and 41:23–41:47:** Subnetting is described as dividing one network into multiple networks by borrowing host bits to save IP addresses.

**Status:** Useful introductory definition, but it is incomplete if address saving is presented as the only reason.

**More complete version:** Subnetting divides an address block into smaller subnets by extending the network prefix (borrowing bits from the host portion). It helps allocate address space efficiently and organize Layer 3 networks, VLANs, routing, and broadcast domains.

## 10. Formula for required subnets

**Transcript around 27:33–35:08:** `2^n >= x`, where n is the number of borrowed bits and x is the number of required subnets.

**Status:** Correct for equal-size subnetting from a given parent prefix.

**Nuance:** In the example, 3 required subnets means borrowing 2 bits, which produces 4 equal-size subnets. For real designs, variable-length subnet masking (VLSM) may allocate different subnet sizes based on each team's needs.

## 11. /26 calculation

**Transcript around 33:34–38:56:** Borrowing two bits from /24 gives /26, mask `255.255.255.192`, 6 host bits, and blocks of 64 addresses.

**Status:** Correct. Each conventional /26 has 64 total addresses and 62 usable host addresses.

The ranges are:
- `192.168.1.0/26`: hosts .1–.62, broadcast .63
- `192.168.1.64/26`: hosts .65–.126, broadcast .127
- `192.168.1.128/26`: hosts .129–.190, broadcast .191
- `192.168.1.192/26`: hosts .193–.254, broadcast .255

## 12. “Move from Class A to Class B” for 300 devices

**Transcript around 47:47–48:28:** The example suggests moving to a Class B network if a team grows to 300 members.

**Issue:** This is an outdated classful way to describe modern address planning and would often allocate far more space than necessary.

**Correct version:** Choose a prefix based on the required number of hosts and the surrounding address plan. A /24 has only 254 traditionally usable host addresses; a /23 has 510 and can support 300 hosts in one subnet. Whether /23 is appropriate depends on the design, available address space, broadcast-domain size, and growth needs. It is not automatically necessary to choose a classful Class B network.

## 13. Default gateway

**Transcript around 56:14:** The example names `192.168.0.1` as the default gateway.

**Clarification:** A default gateway is the router or Layer 3 interface a host uses to reach destinations outside its local subnet. Its address is selected by the network design; it is often .1 or .254 by convention, but neither address is mandatory.

## 14. Transcript uncertainty

Several lines in the provided transcript contain garbled octets, repeated numbers, missing words, or phrases that do not form a technically clear statement (for example around 16:30–18:19 and 37:17–38:42). Where the intended values can be verified from the worked example, the notes use the correct /26 ranges. Where the meaning cannot be recovered confidently, it is not presented as a definite instructor quote.

## Bottom line

The main teaching sequence is useful: calculate a subnet ID, understand the on-link/off-link decision, determine how many subnet bits are needed, then calculate the subnet ranges and usable host count. The corrections above keep the concepts accurate without silently rewriting the original lesson.
