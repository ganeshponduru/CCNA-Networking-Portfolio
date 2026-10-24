# SSH vs. Telnet and Packet Observations

## SSH and Telnet compared

| Item | SSH | Telnet |
|---|---|---|
| Common TCP port | 22 | 23 |
| Typical use | Secure remote CLI management | Legacy remote terminal access |
| Confidentiality | Encrypts the SSH session after protocol negotiation | Sends terminal data without encryption |
| Credentials/data visibility | Protected in transit by SSH encryption when correctly negotiated | Can be visible in a packet capture |
| Recommendation | Preferred for remote management | Avoid on untrusted networks; use only in controlled legacy labs when necessary |

Both SSH and Telnet use TCP, so both begin with a TCP three-way handshake. A successful handshake only proves that the TCP connection was established; it does not prove that login authentication succeeded.

## What I learned

I studied a two-router example for connecting to a peer's SSH service and comparing it with Telnet. I also reviewed packet examples showing the TCP handshake and flags.

## Interpreting packet captures

- SSH: identify TCP traffic to port 22, observe the handshake, then recognize that SSH application payload is encrypted. Packet metadata such as IP addresses, ports, packet sizes, timing, and direction may still be visible.
- Telnet: identify TCP traffic to port 23. Terminal input may appear as readable payload in a capture, including sensitive data. Do not capture real credentials or connect to production devices for this exercise.
- Refused Telnet attempt: a TCP RST may be returned if no service is listening or a device actively rejects the connection. A firewall may instead silently drop traffic or respond differently. A RST alone does not identify the exact cause.

## TCP teardown

A normal TCP close commonly includes FIN/ACK exchanges as each direction closes. An abrupt reset may use RST. Do not assume every FIN means the entire connection closed in one packet or that every abrupt close must show a RST.

## Safe lab practice

Use only your own isolated GNS3, EVE-NG, Packet Tracer where supported, or authorized lab devices. Do not use real passwords in captures, screenshots, or GitHub notes. If documenting a capture, redact public addresses or identifiers when they could expose a real network.
