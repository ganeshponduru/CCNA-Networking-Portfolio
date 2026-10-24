# CCNA Networking Portfolio

I'm building this repository as I learn networking and prepare for CCNA and entry-level Network Support/TAC roles. It contains my study notes, worked examples, revision questions, and evidence from labs I have actually completed.

## How I organize my learning

The [fundamentals folder](fundamentals/README.md) contains Days 01–09 in chronological order. Each day has its own folder so related notes, diagrams, screenshots, and lab observations stay together.

I keep theory and practical work separate. I only mark a lab as completed after I perform it and record what I configured or observed. Study notes and planned exercises are labelled honestly rather than presented as completed labs.

## My goal

I want to explain networking concepts in my own words, solve addressing problems without memorising answers, and gradually add practical configuration and troubleshooting evidence as I build my skills.

## Learning roadmap

| Day | Topics | Current documentation |
|---|---|---|
| [Day 01](fundamentals/day-01/README.md) | How data moves, OSI and TCP/IP, encapsulation, TCP/UDP, common ports, packet headers | Study notes, diagrams, and a Wireshark practice plan |
| [Day 02](fundamentals/day-02/README.md) | Switching, traffic types, network domains, topologies, IPv4 basics, binary conversion | Study notes and revision questions |
| [Day 03](fundamentals/day-03/README.md) | IPv4 subnetting, network IDs, subnet masks, and /26 calculations | Notes, worked examples, and correction log |
| [Day 04](fundamentals/day-04/README.md) | Network and broadcast addresses, subnet ranges, subnet design | Worked calculations and practice questions |
| [Day 05](fundamentals/day-05/README.md) | ARP, ICMP, ping, and inter-subnet routing | Lab notes, packet observations, and screenshots |
| [Day 06](fundamentals/day-06/README.md) | DNS, TCP handshake and flags, packet analysis | Wireshark case study and screenshots |
| [Day 07](fundamentals/day-07/README.md) | Networking hardware, fiber, transceivers, physical-link troubleshooting, console access | Study notes and troubleshooting checklist; hardware and console labs are not claimed as completed |
| [Day 08](fundamentals/day-08/README.md) | TCP handshake, SSH, Telnet, and Cisco IOS CLI | Configuration notes, packet observations, and screenshots |
| [Day 09](fundamentals/day-09/README.md) | DHCP, DORA, DHCP options, and Wireshark packet analysis | DHCP lab notes and R1–Switch1 / R2–Switch1 screenshots |

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

## Day 05 — Ping, ARP, and ICMP

- [Day 5 lab overview and evidence](fundamentals/day-05/README.md)
- [ARP fundamentals](fundamentals/day-05/arp-fundamentals.md)
- [ICMP and ping](fundamentals/day-05/icmp-and-ping.md)
- [Different-subnet routing lab](fundamentals/day-05/different-subnet-routing-lab.md)
- [Packet-level walkthrough](fundamentals/day-05/packet-level-walkthrough.md)
- [Revision questions](fundamentals/day-05/practice-questions.md)

## Day 06 — DNS and TCP

- [Day 6 overview](fundamentals/day-06/README.md)
- [DNS resolution and record types](fundamentals/day-06/dns-resolution.md)
- [DNS and TCP Wireshark case study](fundamentals/day-06/dns-tcp-wireshark-case-study.md)
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

## Day 08 — TCP, SSH, Telnet, and Cisco CLI

- [Day 8 overview](fundamentals/day-08/README.md)
- [TCP three-way handshake and flags](fundamentals/day-08/tcp-handshake-and-flags.md)
- [SSH configuration on Cisco IOS](fundamentals/day-08/ssh-configuration.md)
- [SSH vs. Telnet and packet observations](fundamentals/day-08/ssh-vs-telnet.md)
- [SSH and Telnet Wireshark case study](fundamentals/day-08/ssh-telnet-wireshark-case-study.md)
- [Cisco CLI modes and interface basics](fundamentals/day-08/cisco-cli-basics.md)
- [Revision questions](fundamentals/day-08/practice-questions.md)

## Day 09 — DHCP and DORA packet analysis

- [Day 9 overview](fundamentals/day-09/README.md)
- [DHCP DORA packet-level walkthrough](fundamentals/day-09/dora-packet-walkthrough.md)
- [DHCP options and header fields](fundamentals/day-09/dhcp-options-and-fields.md)
- [DHCP configuration and verification checklist](fundamentals/day-09/configuration-and-verification.md)
- [R1–Switch1 packet details](fundamentals/day-09/capture-r1-switch.md)
- [R2–Switch1 packet details](fundamentals/day-09/capture-r2-switch.md)
