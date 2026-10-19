# Day 6 — Packet Analysis Checklist

I use this checklist to keep my packet analysis structured instead of guessing from one packet.

## DNS lookup

- [ ] I selected the network interface that carries my traffic.
- [ ] I generated a lookup for a domain name.
- [ ] I used the display filter dns.
- [ ] I identified the query and matching response.
- [ ] I checked the query name and query type (A, AAAA, or another type).
- [ ] I checked source/destination IP addresses and transport ports.
- [ ] I checked whether the response contains an answer, an error, or a referral.
- [ ] I noted whether caching could explain why no new DNS query appeared.
- [ ] I saved a screenshot or capture only after performing the test myself.

## TCP connection

- [ ] I generated a new connection to a service I am allowed to test.
- [ ] I identified the client and server IP addresses.
- [ ] I checked the destination port.
- [ ] I found the SYN, SYN-ACK, and ACK, if the handshake completed.
- [ ] I checked for retransmissions, resets, or missing replies if it did not complete.
- [ ] I separated observations from conclusions.
- [ ] I did not assume that a red packet colour alone tells me the cause; Wireshark colouring depends on its rules.

## What I would write in a lab report

**Objective:** What I wanted to verify.

**Setup:** Device, interface, target, and relevant addressing.

**Method:** Commands or actions I performed.

**Observation:** The exact packets, fields, responses, or errors I saw.

**Conclusion:** What the evidence supports and what it does not prove.

**Next test:** What I would check next if the result were unexpected.

## Evidence status

This file is a checklist, not proof that I have completed each exercise. I will tick items only after running the test and recording my own result.
