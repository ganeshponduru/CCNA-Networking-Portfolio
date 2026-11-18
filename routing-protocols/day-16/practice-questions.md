# Practice and Interview Questions

## Questions and Answers

### 1. What is the purpose of traceroute?

Traceroute identifies the routers between a source and a destination. It also displays the round-trip time for probes sent to each responding hop. It is useful when checking where a path changes or where responses stop.

### 2. What is the difference between traceroute and tracert?

The common Linux/Unix traceroute command uses UDP probes by default. Windows tracert uses ICMP Echo Request probes. Both increase the TTL to discover the hops along the path.

### 3. What is TTL?

TTL means Time to Live. It is an 8-bit field in the IPv4 header. Routers reduce it when forwarding a packet. If it expires, the packet is dropped and an ICMP Time Exceeded message is normally returned.

### 4. How does TTL prevent routing loops?

If a packet keeps circulating because of a routing loop, each router reduces its TTL. When the value expires, the packet is discarded instead of continuing around the loop forever.

### 5. Which ICMP message is generated when TTL expires in transit?

ICMP Type 11, Code 0 — Time Exceeded.

### 6. Which ICMP response commonly marks the end of UDP traceroute?

ICMP Type 3, Code 3 — Destination Unreachable, Port Unreachable. It commonly means the destination IP was reached but the UDP destination port used by the probe was closed.

### 7. Why does traceroute send three probes for each TTL value?

The common default is three probes per TTL. They provide multiple RTT measurements for the same hop and make it easier to notice inconsistent or missing responses. They are three probes of the same method, not three different packet types.

### 8. What UDP destination ports are used by the traceroute method covered here?

The PDF gives the range 33434–33523. The destination port is varied across probes.

### 9. What is RTT?

Round Trip Time is the time from sending a probe until its response returns to the source. Traceroute reports an RTT for each probe that receives a response.

### 10. What does * * * mean in traceroute?

It means no response was received for those probes before the timeout. Possible reasons include packet filtering, ICMP rate limiting, packet loss, a delayed response, or a routing problem. It does not automatically mean the router is down.

### 11. If one hop shows * * * but later hops respond, what does that tell you?

The router at that hop may be forwarding traffic without replying to the traceroute probes. The later responses show that the probes continued farther along the path.

### 12. Does a successful ping guarantee that traceroute will work?

No. Ping and traceroute can use different probe types. A network may allow ICMP Echo while filtering UDP probes, or it may handle ICMP Time Exceeded replies differently.

### 13. Does Type 3, Code 3 mean the destination application is reachable?

No. It means the destination returned Port Unreachable for the UDP probe. It confirms that the destination IP responded, but it does not prove that a particular application service is available.

### 14. Can traceroute identify the exact physical cable or device failure?

No. It shows where probes stop receiving responses. Further checks, such as routing-table inspection, interface status, logs, and access to network devices, are needed to identify the actual fault.

### 15. Why can RTT values differ for the same hop?

Each probe is a separate packet. Queueing, congestion, processing delay, and packet loss can cause different RTT measurements.

## Quick Revision

| Item | Value / meaning |
|---|---|
| IPv4 TTL field size | 8 bits |
| Maximum TTL value | 255 |
| TTL expired in transit | ICMP Type 11, Code 0 |
| UDP port unreachable | ICMP Type 3, Code 3 |
| Common UDP traceroute destination ports in the PDF | 33434–33523 |
| Common probes per TTL | 3 |
| RTT | Probe-to-response round-trip time |
