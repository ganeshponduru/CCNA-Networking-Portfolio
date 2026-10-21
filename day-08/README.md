# Day 8 — TCP Handshake, SSH, Telnet, and Cisco CLI Basics

These are my notes from the Day 8 online class and the supplied SSH Configuration PDF.

## Notes

- [TCP three-way handshake and flags](tcp-handshake-and-flags.md)
- [SSH configuration on Cisco IOS](ssh-configuration.md)
- [SSH vs. Telnet and packet observations](ssh-vs-telnet.md)
- [Cisco CLI modes and interface basics](cisco-cli-basics.md)
- [Revision questions](practice-questions.md)

## What I studied

- TCP connection establishment with SYN, SYN-ACK, and ACK.
- TCP flags including SYN, ACK, PSH, FIN, RST, and URG.
- SSH remote management over TCP port 22 and Telnet over TCP port 23.
- Basic Cisco IOS CLI modes, interface addressing, `no shutdown`, and verification commands.
- The main steps for enabling SSH using a hostname, domain name, RSA keys, VTY lines, and local user authentication.

## Important cautions

- Never publish real usernames, passwords, private keys, or production-device details. The example credentials in the handout are intentionally omitted from these notes.
- The PDF's 1024-bit RSA example is kept out of the recommended configuration; use a key size supported by the platform and current security policy, commonly at least 2048 bits.
- A TCP handshake establishes the TCP connection; SSH then performs its own key exchange and authentication before protected session data is exchanged.
- PSH does not mean “urgent data.” URG is a separate flag, and neither flag should be described as a guarantee that an application receives data instantly.
- A TCP RST can indicate a refused or aborted connection, but it does not by itself prove that a port is closed; inspect the context and device/firewall behavior.
