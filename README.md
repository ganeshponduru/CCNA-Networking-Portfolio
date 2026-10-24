# CCNA Networking Portfolio

I'm building this repository as I learn networking and prepare for CCNA and entry-level network support/TAC roles. I use it to keep my notes, worked examples, revision questions, and evidence from labs I actually complete.

## Learning roadmap

| Day | What I studied | Status |
|---|---|---|
| Day 01 | How data moves, OSI and TCP/IP models, encapsulation, TCP/UDP, common ports, packet headers | Notes added; Wireshark practice still to be completed and documented |
| Day 02 | Circuit and packet switching, traffic types, network domains, topologies, IPv4 basics, binary conversion | Notes added; hands-on evidence not yet added |
| Day 03 | IPv4 subnetting, network IDs, subnet masks, and /26 calculations | Notes and worked examples added; hands-on evidence not yet added |
| Day 04 | Network and broadcast address calculations, next subnet ranges, and subnet design | Notes and worked examples added; hands-on evidence not yet added |
| Day 05 | Ping, ICMP, ARP, and Wireshark packet inspection | Same-subnet lab completed; screenshots and observations documented |
| Day 06 | DNS resolution, DNS records, Wireshark DNS analysis, TCP three-way handshake, and TCP flags | Notes and screenshot-based DNS/TCP/SSH packet observations documented; further independent captures can be added as practice |
| Day 07 | Networking hardware, fiber optics, transceivers, chassis modules, physical-link troubleshooting, and console access | Study notes and troubleshooting checklist added; hardware and console labs not claimed as completed |
| Day 08 | TCP handshake and flags, SSH vs. Telnet, SSH configuration, and Cisco IOS CLI basics | Notes and configuration reference added |
| Day 09 | DHCP fundamentals, DORA, DHCP options, and Wireshark packet analysis | DHCP lab completed; notes and R1–Switch1 and R2–Switch1 screenshots added |

## Day 01 — How data moves across a network

- [Day 1 overview](fundamentals/day-01/README.md)

- [OSI and TCP/IP models](fundamentals/day-01/osi-vs-tcp-ip.md)
- [Encapsulation and decapsulation](fundamentals/day-01/encapsulation-decapsulation.md)
- [TCP vs UDP](fundamentals/day-01/tcp-vs-udp.md)
- [Common protocol ports](fundamentals/day-01/common-protocol-ports.md)
- [Understanding packet headers](fundamentals/day-01/packet-header-overview.md)
- [Revision questions](fundamentals/day-01/interview-questions.md)
- [Wireshark practice plan](fundamentals/day-01/packet-analysis/README.md)
- [References I used](fundamentals/day-01/references.md)
- [OSI/TCP-IP diagram](fundamentals/day-01/diagrams/osi-tcpip-mapping.svg)
- [Encapsulation diagram](fundamentals/day-01/diagrams/encapsulation-flow.svg)

## Day 02 — Switching and IPv4 basics

- [Day 2 notes](fundamentals/day-02/README.md)
- [Circuit switching and packet switching](fundamentals/day-02/switching.md)
- [Unicast, broadcast, and multicast](fundamentals/day-02/traffic-types.md)
- [Collision and broadcast domains](fundamentals/day-02/domains.md)
- [Network topologies](fundamentals/day-02/topologies.md)
- [IPv4 addressing and private ranges](fundamentals/day-02/ipv4-addressing.md)
- [Binary and decimal conversion](fundamentals/day-02/binary-conversion.md)
- [Revision questions](fundamentals/day-02/interview-questions.md)

## Day 03 — Subnetting

- [Day 3 notes](fundamentals/day-03/README.md)
- [Subnetting fundamentals](fundamentals/day-03/subnetting-fundamentals.md)
- [Worked example: four /26 subnets](fundamentals/day-03/worked-example.md)
- [Things I corrected or clarified](fundamentals/day-03/correction-log.md)
- [Revision questions](fundamentals/day-03/interview-questions.md)

## Day 04 — Subnet calculations and subnet design

- [Day 4 notes](fundamentals/day-04/README.md)
- [Finding network, broadcast, and usable addresses](fundamentals/day-04/address-range-calculations.md)
- [Finding the next subnet and planning subnets](fundamentals/day-04/next-subnet-and-subnet-design.md)
- [Practice questions and answers](fundamentals/day-04/practice-questions.md)
- [Points I clarified while studying](fundamentals/day-04/things-i-checked.md)

## Day 08 — TCP, SSH, Telnet, and Cisco CLI

- [Day 8 overview](fundamentals/day-08/README.md)
- [TCP three-way handshake and flags](fundamentals/day-08/tcp-handshake-and-flags.md)
- [SSH configuration on Cisco IOS](fundamentals/day-08/ssh-configuration.md)
- [SSH vs. Telnet and packet observations](fundamentals/day-08/ssh-vs-telnet.md)
- [Cisco CLI modes and interface basics](fundamentals/day-08/cisco-cli-basics.md)
- [Revision questions](fundamentals/day-08/practice-questions.md)

## How I document my progress

I keep theory, calculations, and hands-on work separate. I only mark a lab as completed after I perform it and record what I configured and observed. Planned exercises are labelled as planned, not presented as completed.

## My goal

I want to explain these concepts in my own words, solve addressing problems without memorising answers, and gradually add practical configuration and troubleshooting evidence as I build my skills.

## Day 05 — Ping, ARP, and ICMP

- [Day 5 lab overview and evidence](fundamentals/day-05/README.md)
- [ARP fundamentals](fundamentals/day-05/arp-fundamentals.md)
- [ICMP and ping](fundamentals/day-05/icmp-and-ping.md)
- [Revision questions](fundamentals/day-05/practice-questions.md)


## Day 06 — DNS and TCP

- [Day 6 overview](fundamentals/day-06/README.md)
- [DNS resolution and record types](fundamentals/day-06/dns-resolution.md)
- [TCP three-way handshake and flags](fundamentals/day-06/tcp-handshake-and-flags.md)
- [Packet-analysis checklist](fundamentals/day-06/packet-analysis-checklist.md)
- [Revision questions](fundamentals/day-06/practice-questions.md)


## Day 07 — Networking hardware and physical connectivity

- [Day 7 overview](fundamentals/day-07/README.md)
- [Networking hardware components](fundamentals/day-07/hardware-components.md)
- [Fiber optics, transceivers, and connectors](fundamentals/day-07/fiber-optics-and-transceivers.md)
- [Chassis, line cards, and redundancy](fundamentals/day-07/chassis-and-modular-hardware.md)
- [Physical-link troubleshooting checklist](fundamentals/day-07/physical-link-troubleshooting.md)
- [Console access basics](fundamentals/day-07/console-access-basics.md)
- [Revision questions](fundamentals/day-07/practice-questions.md)


## Day 09 — DHCP and DORA packet analysis

- [Day 9 overview](fundamentals/day-09/README.md)
- [DHCP DORA packet-level walkthrough](fundamentals/day-09/dora-packet-walkthrough.md)
- [DHCP options and header fields](fundamentals/day-09/dhcp-options-and-fields.md)
- [DHCP configuration and verification checklist](fundamentals/day-09/configuration-and-verification.md)
