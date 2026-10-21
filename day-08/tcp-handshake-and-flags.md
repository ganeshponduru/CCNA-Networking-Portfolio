# TCP Three-Way Handshake and Flags

## 1. Why the handshake happens

TCP establishes connection state between two endpoints before normal application data is exchanged. For a browser connecting to an HTTPS service that uses TCP, the destination port is commonly 443. The client uses an ephemeral source port.

Not every modern website connection uses TCP: HTTP/3 uses QUIC over UDP, commonly UDP port 443. The TCP example in this lesson applies to TCP-based services such as HTTPS over TCP and SSH.

## 2. Three-way handshake

| Step | Direction | Flags | Meaning |
|---|---|---|---|
| 1 | Client → server | SYN | Client requests to establish a TCP connection and supplies its initial sequence number. |
| 2 | Server → client | SYN, ACK | Server acknowledges the client's SYN and supplies its own initial sequence number. |
| 3 | Client → server | ACK | Client acknowledges the server's SYN. The TCP connection is established. |

For an SSH connection, the client normally sends a SYN to destination TCP port 22. For a TCP-based HTTPS connection, the destination is normally TCP port 443. The source port is selected from the client's ephemeral port range.

## 3. TCP flags covered in the lesson

- **SYN** — used to synchronize sequence numbers and establish a TCP connection.
- **ACK** — acknowledges received TCP sequence space. ACK is set on most segments after the initial handshake.
- **PSH** — asks the receiving TCP implementation to push buffered data toward the receiving application. It does not mean “urgent data,” and it is not a guarantee of instant delivery.
- **FIN** — indicates that the sender has finished sending data on its direction of the connection. TCP can close each direction independently, so orderly teardown may use FIN exchanges in both directions.
- **RST** — resets a TCP connection or rejects an attempt in some circumstances. Interpret it with the surrounding packets and endpoint behavior.
- **URG** — indicates that the urgent pointer field is significant. It is not a general-purpose “send this immediately” flag and is uncommon in modern application traffic.

## 4. What to check in a packet capture

1. Identify the client and server IP addresses.
2. Confirm the TCP source and destination ports.
3. Find the initial SYN.
4. Check for a matching SYN-ACK from the server.
5. Check for the final ACK from the client.
6. Inspect later packets for application data and teardown/reset behavior.
7. If the expected response is missing, consider filtering, packet loss, routing, host firewall behavior, service state, and capture location before concluding why.

Useful Wireshark display filters:

- `tcp`
- `tcp.flags.syn == 1`
- `tcp.flags.syn == 1 && tcp.flags.ack == 0` — initial SYNs
- `tcp.flags.fin == 1`
- `tcp.flags.reset == 1`
- `tcp.port == 22` — SSH-related TCP traffic
- `tcp.port == 23` — Telnet-related TCP traffic
- `tcp.port == 443` — TCP-based HTTPS traffic

## 5. Important distinction

The three-way handshake creates TCP connection state. It does **not** complete the SSH login or prove that an HTTPS application request succeeded. SSH still needs its protocol negotiation, cryptographic key exchange, and authentication. HTTPS also performs TLS negotiation before protected application data is exchanged.
