# Day 8 — SSH and Telnet Wireshark Observations

## Topology

The topology contains two routers connected through GigabitEthernet1/0:

- **R1 (HYD):** `192.168.1.1`
- **R2 (BLR):** `192.168.1.2`

![Two-router topology](./Screenshot%202026-10-22%20124703.png)

## SSH packet observations

The screenshots show a TCP connection from `192.168.1.2` to `192.168.1.1` using client source port `56822` and destination port `22`.

![SSH TCP handshake packets](./Screenshot%202026-10-22%20121618.png)

| Packet | Observed flags | What it shows |
|---|---|---|
| Packet 15 | SYN | The client starts the TCP connection to SSH port 22. |
| Packet 16 | SYN, ACK | The peer acknowledges the SYN and responds with its own SYN. |
| Packet 17 | ACK | The client acknowledges the peer's SYN; the TCP handshake is complete. |
| Packet 19 | PSH, ACK | TCP carries data for the established connection. PSH does not mean “urgent data.” |
| Packet 128 | SSHv2 | Wireshark identifies SSH protocol data; the displayed SSH payload is encrypted. |

![SSH data and TCP flags](./Screenshot%202026-10-22%20121735.png)

The TCP handshake is only the start of the SSH connection. SSH protocol negotiation, key exchange, and authentication happen after the TCP connection is established.

## Telnet packet observations

The screenshots show a connection attempt from `192.168.1.2` to `192.168.1.1`, destination TCP port `23`.

- Packet 525 shows a SYN from client port `51451` to TCP port `23`.
- Packet 526 shows `RST, ACK` from TCP port `23` back to client port `51451`. This indicates that the connection attempt was reset or rejected. The RST alone does not establish the exact reason.

![Telnet connection attempt and RST ACK](./Screenshot%202026-10-22%20122455.png)

- Later packets show Telnet data exchanged between `192.168.1.1` and `192.168.1.2`, including readable one-byte payload `a` in packet 575. This illustrates that Telnet terminal data is not encrypted.

![Telnet readable payload](./Screenshot%202026-10-22%20122832.png)

## TCP flags reviewed

- **SYN:** starts connection establishment.
- **SYN, ACK:** acknowledges the client's SYN and sends the peer's SYN.
- **ACK:** acknowledges TCP sequence space.
- **PSH, ACK:** carries data with the PSH flag set; it does not mean the data is urgent.
- **RST, ACK:** resets/rejects a connection in this packet context.
- **FIN, PSH, ACK:** packet 333 shows FIN and PSH together with ACK. FIN indicates the sender is closing its sending direction; PSH is a separate flag.

## Main takeaway

SSH uses TCP port 22 and encrypts its session data. Telnet uses TCP port 23 and sends terminal data without encryption. Wireshark can show TCP flags and connection metadata for both protocols, while Telnet payload may be readable.
