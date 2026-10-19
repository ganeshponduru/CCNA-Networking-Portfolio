# Day 6 — Revision Questions

I use these questions to check whether I can explain the concepts without reading the notes.

## DNS

1. What problem does DNS solve?
2. What is the difference between an A record and an AAAA record?
3. What is a CNAME record used for?
4. What is the purpose of a PTR record?
5. What is the difference between a recursive query from a client and iterative queries made by a resolver?
6. What do root, TLD, and authoritative DNS servers do in a typical uncached lookup?
7. What does the Recursion Desired (RD) flag request?
8. Why might a browser open a site without a new DNS packet appearing in a capture?
9. Which transport ports are used by DNS, and why is it inaccurate to say DNS uses only UDP?
10. If a name resolves to multiple IP addresses, what can I conclude—and what can I not conclude—from that fact alone?

## TCP

11. What are the three steps in the TCP three-way handshake?
12. What is the purpose of SYN, and what does SYN-ACK communicate?
13. What is the difference between FIN and RST?
14. What does PSH indicate, and how is it different from URG?
15. What should I investigate if I see repeated SYN packets but no SYN-ACK?
16. What does a TCP reset from a destination commonly suggest? Why is it not enough by itself to identify the root cause?
17. Which ports are commonly associated with HTTP, HTTPS, SSH, and DNS?
18. Why can I still see IP addresses and ports in an HTTPS capture even when application data is encrypted?

## Practical tasks

- [ ] Capture a DNS query and response for a domain I choose.
- [ ] Identify the query type and the answer record.
- [ ] Capture a fresh TCP handshake to a permitted service.
- [ ] Identify SYN, SYN-ACK, and ACK in Wireshark.
- [ ] Compare a successful handshake with a failed or reset connection, if I can reproduce one safely.
- [ ] Write down what I observed without claiming a cause that the capture does not establish.

I will add answers and screenshots after I have attempted these questions and practical tasks.
