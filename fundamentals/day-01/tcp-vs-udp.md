# TCP vs UDP

These are the differences I need to understand when choosing between TCP and UDP and when explaining what each protocol does not guarantee.
| Feature | TCP | UDP |
|---|---|---|
| Connection model | Connection-oriented | Connectionless; no transport connection setup |
| Delivery / ordering | Reliable, ordered byte-stream delivery using acknowledgments and retransmission | No built-in delivery, ordering, or retransmission guarantee |
| Flow / congestion control | Built in | Not built into UDP itself |
| Data boundaries | Byte stream; application must frame messages | Datagram boundaries are preserved |
| Header size | Minimum 20 bytes without options | 8 bytes |
| Examples | SSH, SMTP, HTTPS over TCP, many file transfers | DNS often uses UDP, DHCP, many real-time applications |

## TCP

TCP provides a reliable, ordered byte stream. It uses sequence numbers, acknowledgments, retransmissions, and flow/congestion control. TCP itself does not encrypt traffic; TLS or another security protocol can provide encryption and authentication.

## UDP

UDP sends independent datagrams without built-in connection setup or retransmission. “Unreliable” means UDP itself does not guarantee delivery or ordering; applications can implement recovery or reliability. UDP is not automatically faster or always preferable—the application requirements and network conditions matter.

## Examples and caveats

- DNS commonly uses UDP and can also use TCP, depending on response size, configuration, and operation.
- HTTP/3 uses QUIC over UDP, commonly destination port 443; HTTP/1.1 and HTTP/2 are commonly carried over TCP.
- Real-time voice/video often uses UDP-based transport, but designs vary.
- IPv4 protocol numbers 6 and 17 identify TCP and UDP respectively; these are not port numbers.

**Interview answer:** Choose based on application requirements. TCP supplies reliable ordered delivery; UDP supplies datagrams without built-in delivery/order guarantees. Applications using UDP must tolerate loss/reordering or implement recovery themselves.