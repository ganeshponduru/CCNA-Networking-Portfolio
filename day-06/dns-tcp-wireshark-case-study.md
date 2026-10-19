# Day 6 Case Study — Reading DNS, TCP, and SSH Traffic in Wireshark

## Goal

I used Wireshark packet details to connect the DNS and TCP concepts I studied with fields visible in captured traffic.

> Evidence note: This write-up describes the screenshots I reviewed. I only claim what is visible in those screenshots; I do not infer unseen steps or configuration.

## 1. DNS A-record query

In one packet, Wireshark shows a DNS query for `monkeytype.com`:

- Client IPv4 address: `10.111.96.206`
- DNS server IPv4 address: `10.111.96.215`
- Transport: UDP
- Source port: `59206`
- Destination port: `53`
- Query type: `A` (IPv4 address)
- DNS transaction ID: `0xa579`
- Recursion Desired (RD): set

The source port is an ephemeral client port; destination port 53 is the conventional DNS server port. The RD flag indicates that the client requests recursive resolution from the DNS server.

## 2. DNS response

The response has the same transaction ID, `0xa579`, and Wireshark marks it as a successful standard query response with no error.

The response contains three A-record answers:

- `104.26.0.151`
- `104.26.1.151`
- `172.67.69.61`

One expanded answer shows a TTL of 261 seconds. The screenshot reports a response time of approximately 279.947 ms.

### What I can conclude

The capture shows a DNS query and matching response containing three IPv4 addresses. Multiple answers alone do not prove why those addresses were returned or whether a particular load-balancing method was used. The screenshots do not establish whether the resolver obtained the answer from cache or contacted authoritative DNS servers.

## 3. TCP SYN and SYN-ACK

The TCP packet details show the SYN flag set in an initial connection packet. Another packet shows source port 443, destination port 50399, and both SYN and ACK set.

The normal TCP connection-opening sequence is:

1. Client → server: `SYN`
2. Server → client: `SYN, ACK`
3. Client → server: `ACK`

The SYN packet shown has no TCP payload. TCP sequence and acknowledgement values in Wireshark may be displayed as relative numbers, so I check whether relative sequence numbering is enabled before interpreting the displayed values.

## 4. TCP data over port 443

Another packet shows:

- Source port: `50399`
- Destination port: `443`
- TCP payload length: `1320` bytes
- ACK flag set

This indicates that the TCP segment carries data. Port 443 is commonly used for HTTPS, but port number alone does not prove that the payload is encrypted or identify the exact application protocol. In a typical HTTPS session, TLS protects application data after the TLS handshake.

## 5. SSH connection between two lab hosts

A separate Wireshark view shows traffic between `192.168.1.1` and `192.168.1.2` using TCP port 22. The packet list includes:

- `14337 → 22 [SYN]`
- `22 → 14337 [SYN, ACK]`
- `14337 → 22 [ACK]`
- Packets Wireshark identifies as SSHv2 protocol messages

This is consistent with a TCP connection being established to an SSH service. The visible packets support identifying the handshake and SSH protocol traffic; they do not, by themselves, prove that a user successfully authenticated.

## Filters I can use to repeat the analysis

```text
dns
udp.port == 53
tcp.flags.syn == 1
tcp.port == 443
tcp.port == 22
ip.addr == 192.168.1.1 && ip.addr == 192.168.1.2
```

## What I learned from inspecting the packets

- DNS queries and responses can be matched using the DNS transaction ID.
- DNS uses a client-side source port and commonly targets destination port 53.
- The RD bit shows that the client requested recursive resolution; it does not show the full resolution path.
- TCP flags help identify connection setup, but I should inspect the packet sequence rather than rely on one packet alone.
- TCP port 443 is a useful clue, not proof by itself, that traffic is HTTPS.
- A packet capture supports conclusions about visible packets only. I should clearly separate observations from assumptions.

## Evidence status

The screenshots below are the evidence files uploaded to this folder. I have kept their original filenames and included them here so they render directly in the case study.

### Uploaded Wireshark screenshots

![Wireshark screenshot 1](Screenshot%202026-09-09%20085816.png)

![Wireshark screenshot 2](Screenshot%202026-09-09%20090300.png)

![Wireshark screenshot 3](Screenshot%202026-09-09%20120929.png)

![Wireshark screenshot 4](Screenshot%202026-09-09%20121037.png)

![Wireshark screenshot 5](Screenshot%202026-09-09%20121125.png)

![Wireshark screenshot 6](Screenshot%202026-09-10%20111315.png)
