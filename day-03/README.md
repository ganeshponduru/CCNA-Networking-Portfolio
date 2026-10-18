# Day 03 — IPv4 Subnetting Fundamentals

These notes follow the lesson sequence and keep the instructor's main examples. The correction log calls out inaccurate, oversimplified, or transcript-affected statements.

## In this lesson
- [Subnetting fundamentals and formulas](subnetting-fundamentals.md)
- [Worked example: dividing 192.168.1.0/24 into four /26 subnets](worked-example.md)
- [Correction log](correction-log.md)
- [Review and interview questions](interview-questions.md)

## Lesson summary
The class begins with IPv4 address ranges and network IDs, then explains how a host uses its IP address and subnet mask to decide whether a destination is on the local subnet. It then introduces subnetting as a way to divide an address block into smaller networks based on requirements.

The main example starts with `192.168.1.0/24` and borrows two host bits, producing a `/26` mask and four subnets. Each /26 subnet has 64 total addresses and 62 traditionally usable host addresses.

## Source and scope
Based on the subnetting lesson taught by E. Ramesh Goud at TungaBadra Networks, as provided in the class transcript. These are original study notes, not a verbatim transcript or reproduction of course slides.

**Accuracy note:** The instructor uses classful A/B/C terminology to introduce the concept. Modern networks use CIDR prefixes, so classful defaults should not be treated as the normal method for designing or calculating today's networks. See the correction log for details.

## Practical status
This page documents the lesson and worked calculations. It does not claim that a Packet Tracer, router, switch, or PC lab has been completed. Add lab evidence only after performing and verifying it.
