# TCP Three-Way Handshake and Flags — My Notes

## Why TCP starts with a handshake

I learned that TCP establishes connection state between two endpoints before normal application data is exchanged. For a new TCP connection, the usual handshake is:

    Client                         Server
      | -------- SYN -------------> |
      | <------ SYN, ACK ---------- |
      | -------- ACK -------------> |
      |                             |
      |       Connection ready      |

### Step by step

1. **SYN:** The client requests a TCP connection and sends its initial sequence number.
2. **SYN-ACK:** The server acknowledges the client's sequence number and sends its own initial sequence number.
3. **ACK:** The client acknowledges the server's sequence number. The connection is established and application data can be exchanged.

In a packet capture, the first packet often has SYN set, the second has SYN and ACK set, and the third has ACK set. The exact sequence and any retransmissions can differ if packets are lost or delayed.

## TCP flags I am learning

| Flag | What it means in practice |
|---|---|
| SYN | Synchronizes sequence numbers and starts a connection |
| ACK | Indicates that the acknowledgement field is meaningful; used to acknowledge received sequence space |
| FIN | One endpoint has finished sending data and is closing its sending direction |
| RST | Resets or aborts a connection |
| PSH | Requests prompt delivery of received data to the receiving application; it is not a guarantee of immediate delivery |
| URG | Indicates that the urgent pointer field is significant; this is distinct from PSH |

The common opening sequence is **SYN → SYN-ACK → ACK**. TCP flags are not all just “on/off switches for the whole connection”; they have specific meanings in TCP's state and data handling.

## FIN and RST are different

- **FIN** is used for an orderly TCP shutdown. TCP is full-duplex, so each direction can be closed separately. A normal close often involves FIN and ACK exchanges in both directions, but packet sequences can vary.
- **RST** aborts or rejects a connection. For example, connecting to a closed TCP port may result in a reset. A firewall or other network device may also reject/reset traffic, while other filtering policies may silently drop packets and cause a timeout.

Closing a browser tab does not guarantee that I will see one specific FIN packet from the server. Browsers reuse connections, keep connections alive, use multiple connections, and may close them at different times. I need to inspect the actual capture before concluding what happened.

## Ports and examples

| Service | Common port | Typical transport |
|---|---:|---|
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP with TLS; HTTP/3 uses QUIC over UDP 443 |
| SSH | 22 | TCP |
| DNS | 53 | UDP and TCP, depending on the exchange |

A port number identifies a transport-layer service endpoint; it does not prove that a service is running or reachable. The destination port in an initial TCP SYN is the port the client is trying to reach.

## How I inspect a TCP handshake in Wireshark

1. Start a capture on the correct interface.
2. Generate a new TCP connection to a service I am allowed to access.
3. Use a filter such as tcp or narrow it to a known server IP and port.
4. Find the initial SYN, the SYN-ACK, and the final ACK.
5. Expand **Transmission Control Protocol** and check source/destination ports, flags, sequence numbers, acknowledgement numbers, and retransmission indicators.
6. If the handshake does not complete, note exactly which packet is missing and whether I see a reset, retransmissions, or no response.

For HTTPS, a packet capture can normally reveal connection metadata such as IP addresses, ports, TCP flags, packet sizes, and timing. TLS encrypts application content, but it does not hide every piece of network metadata. HTTP traffic without TLS may expose application content to anyone able to capture it on the path; this is why credentials and sensitive data should not be sent over plain HTTP.

## PSH clarification

The lesson used interactive SSH typing as an example of the PSH flag. I understand that PSH is associated with prompt delivery of received data to the application, but it is not a special “urgent data” flag and it does not guarantee a particular response time. URG is a separate TCP flag, and normal SSH traffic should not be described as urgent data merely because it is interactive.
