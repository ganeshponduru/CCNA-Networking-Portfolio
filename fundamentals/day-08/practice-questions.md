# Day 8 — Revision Questions

Try answering these without looking at the notes first.

1. What are the three steps in the TCP three-way handshake?
2. When a client starts an SSH connection, what are the usual TCP source and destination ports?
3. What is the difference between SYN and ACK?
4. What does the PSH flag indicate, and what does it not guarantee?
5. What is the difference between FIN and RST?
6. What does the URG flag indicate?
7. Why is SSH preferred over Telnet for remote device management?
8. What TCP ports are commonly used by SSH and Telnet?
9. What does `login local` do in Cisco VTY configuration?
10. What is the purpose of `transport input ssh`?
11. Why should a hostname and domain name be set before RSA key generation on some Cisco IOS platforms?
12. What does `no shutdown` do, and why might the interface still remain down?
13. Which command gives a quick summary of interface IP addresses and status?
14. Why does a successful TCP handshake not prove that SSH authentication succeeded?
15. What evidence would you collect before deciding why an SSH connection fails?

## Short answer key

1. SYN, SYN-ACK, ACK.
2. A client ephemeral source port to destination TCP 22.
3. SYN starts/synchronizes a connection; ACK acknowledges TCP sequence space.
4. PSH requests that received data be pushed toward the application; it does not mean urgent data or guaranteed instant delivery.
5. FIN is an orderly close of one direction; RST resets/aborts a connection.
6. It marks the urgent pointer as significant; it is not a generic priority flag.
7. SSH encrypts the session; Telnet sends terminal data without encryption.
8. SSH TCP 22; Telnet TCP 23.
9. It uses the local username database for authentication.
10. It permits incoming SSH on the selected VTY lines.
11. Some Cisco IOS implementations use hostname and domain name to form the device identity for RSA host-key generation.
12. It removes administrative shutdown; link state still depends on the peer, cabling, and other conditions.
13. `show ip interface brief`.
14. SSH still needs protocol negotiation, key exchange, and authentication after TCP is established.
15. Interface/IP state, route/reachability, TCP handshake, SSH service/VTY/user settings, ACL/firewall behavior, and relevant client/server errors.
