# Wireshark Packet-Analysis Exercise — Template

**Status: Planned exercise.** This is a worksheet, not evidence that a capture has already been performed. Complete it after running Wireshark and save your own screenshots/capture where appropriate.

## Objective

Observe protocol headers in a packet capture and relate Ethernet, IP, TCP/UDP, and application fields to Day 1 concepts.

## Procedure

1. Install Wireshark from its official website and select the interface carrying your test traffic.
2. Capture only on a device/network you own or are authorized to monitor.
3. Generate ordinary traffic to a permitted service, then stop the capture.
4. Try display filters: dns, tcp, udp, tcp.port == 443, or ip.addr == YOUR_TEST_IP (replace the placeholder).
5. Select a packet and expand Ethernet, IP, TCP/UDP, and application protocol details.

## Record your evidence

- Date / environment:
- Interface captured:
- Traffic generated:
- Display filter:
- Packet number and protocol:
- Ethernet source / destination MAC:
- IP source / destination:
- Transport protocol and source / destination ports:
- Relevant TCP flags or DNS fields:
- What the fields indicate:
- What cannot be concluded from this packet alone:
- Screenshot filenames:
- PCAP filename (remove sensitive data before sharing):

## Analysis prompts

1. For a remote IP destination, is the Ethernet destination the remote host or the local next hop?
2. Which IPv4 header field identifies TCP versus UDP?
3. Are you seeing a DNS query, response, or both?
4. With encrypted traffic, what metadata remains visible and what payload details are unavailable?
5. What additional packets/logs would be needed before concluding a root cause?

## Evidence and privacy

Do not upload captures containing credentials, private messages, customer information, or other sensitive data. Anonymize details. Add actual findings and change the status from Planned only after personally performing and documenting the exercise.