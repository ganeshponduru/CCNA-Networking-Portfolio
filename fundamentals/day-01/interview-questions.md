# Day 1 — Interview and Revision Questions

I use these questions to check whether I can explain the basic networking concepts in my own words.
1. **What is the purpose of a reference model?** It structures communication functions and helps organize troubleshooting.
2. **How many layers are in OSI?** Seven: Physical, Data Link, Network, Transport, Session, Presentation, Application.
3. **What are the four common TCP/IP layers?** Application, Transport, Internet, Network Access/Link.
4. **What is encapsulation?** Adding protocol-specific control information as data moves down the stack.
5. **What is decapsulation?** Processing/removing protocol wrappers as data moves up the receiving stack.
6. **MAC vs IP address?** MAC is used for link-layer delivery on a local segment; IP supports logical addressing/routing between networks.
7. **Does the local Ethernet frame use the remote server's MAC?** Usually not; for a remote destination, it normally uses the next-hop router's MAC.
8. **TCP vs UDP?** TCP provides reliable ordered byte-stream delivery; UDP has no built-in delivery/order guarantees.
9. **Does TCP encrypt traffic?** No. TLS or another security protocol can provide encryption.
10. **What are IPv4 protocol numbers 6 and 17?** TCP and UDP. These are not port numbers.
11. **Common ports for SSH, Telnet, DNS, HTTP, HTTPS?** TCP 22, TCP 23, UDP/TCP 53, TCP 80, and commonly TCP 443; UDP 443 is used by QUIC/HTTP/3.
12. **What can Wireshark show?** Captured frames and decoded fields, subject to capture point, filters, encryption, and available data.
13. **Why might DNS use TCP?** DNS supports TCP as well as UDP, including for some large responses and zone transfers.
14. **What would you check when a website is unreachable?** Scope/symptoms, link/VLAN, IP/subnet/gateway, routing, DNS, transport reachability, and application/TLS behavior—validating each step with evidence.

## Self-assessment

Answer these from memory and revisit any that you cannot explain clearly. These are revision prompts, not evidence of interview experience.