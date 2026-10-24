# My Wireshark Packet-Analysis Practice

**Status: Planned — not completed yet.** This page is my checklist for a future capture, not evidence that I have already run Wireshark or inspected packets. I'll update it after I perform the exercise and record my own observations.

## Goal

I want to inspect a small packet capture and connect the fields I see to the networking concepts in my notes: Ethernet framing, IP addressing, TCP/UDP ports, and application protocols.

## Planned steps

1. Install Wireshark from its official website and choose the interface carrying my test traffic.
2. Start a capture and generate simple traffic, such as opening a website or running a DNS lookup.
3. Stop the capture and apply a filter such as `dns`, `tcp`, `udp`, or `icmp`.
4. Select a packet and expand the Ethernet, IP, transport, and application protocol sections.
5. Record the source and destination addresses, protocol, ports where applicable, and any flags or response codes I can identify.
6. Save screenshots that show my own capture and the fields I describe. Avoid publishing private addresses, usernames, tokens, or other sensitive data.

## Notes I will record after the lab

- What traffic I generated:
- Capture interface:
- Display filter used:
- Example packet and protocol:
- Fields I identified:
- What I learned from the capture:
- Any question I still need to investigate:

## What I expect to practise

- Ethernet source and destination MAC addresses
- Source and destination IPv4 addresses
- TCP or UDP source and destination ports
- The difference between a protocol number in an IP header and a transport-layer port
- How the visible packet fields relate to encapsulation

I'll replace this planned status with actual results only after I complete the capture.
