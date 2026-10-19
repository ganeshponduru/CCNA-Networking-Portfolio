# Day 6 — DNS, TCP Handshake, and Packet Analysis

Today I studied how a computer resolves a domain name, how a DNS query appears in Wireshark, and how TCP establishes and closes connections.

## What I learned

- [DNS basics and resolution flow](dns-resolution.md)
- [TCP three-way handshake and control flags](tcp-handshake-and-flags.md)
- [Packet-analysis and troubleshooting checklist](packet-analysis-checklist.md)
- [Revision questions](practice-questions.md)

## My key takeaways

1. DNS helps translate names into records such as IPv4 and IPv6 addresses.
2. My device normally sends DNS queries to the recursive resolver configured on the network.
3. A resolver can use root, TLD, and authoritative DNS servers when it does not already have an answer cached.
4. DNS commonly uses UDP port 53, but it can also use TCP port 53.
5. For a TCP connection, the usual opening sequence is SYN, SYN-ACK, ACK.
6. TCP flags help manage connection setup, data handling, and connection closure.
7. Packet captures help me compare what I expect to happen with the traffic that is actually visible.

## Practice status

These notes are based on the lesson transcript and my study. I will mark a Wireshark exercise as completed only after I run my own capture and save my own evidence. The live demonstration described in the lesson is not being presented here as my personal capture.

## Points I clarified while studying

- DNS is a distributed system, not one server per domain. A domain can have multiple records, servers, and changing answers.
- There are 13 named DNS root-server identities, not just 13 physical machines. Each identity is served by many instances around the world, commonly using anycast.
- A DNS response with multiple IP addresses does not by itself prove that load balancing is the reason.
- A TCP RST can indicate a refused or aborted connection, but the exact cause needs more evidence.
- The PSH flag is not the same as TCP urgent data; URG is a separate flag.
- HTTPS usually uses TLS over TCP port 443, while HTTP/3 uses QUIC over UDP port 443.
