# Reading Traceroute Output and Troubleshooting

## Reading the output

A traceroute result normally displays one line for each TTL value (hop). The line contains the hop number, the responding router address or name, and the RTT measurements for the probes.

For example, if the output shows:

- Hop 1 — 192.168.45.4
- Hop 2 — 192.168.24.2
- Hop 3 — 192.168.12.1

then those are the addresses that responded at TTL 1, TTL 2, and TTL 3. The address shown is the source address used in that router's ICMP response. It is not necessarily a router ID or loopback address.

## What does * * * mean?

An asterisk means that traceroute did not receive a response for that probe before its timeout. It does **not** prove by itself that the router is down or that there is no route.

Possible reasons include:

1. The router has no route to the destination.
2. UDP probes or ICMP replies are filtered by an ACL or firewall.
3. The router does not send, or rate-limits, the ICMP response.
4. A probe or response was lost or delayed.
5. The forward or return path has a problem.

The session demonstrated two possibilities to check first: a missing route and UDP traffic being blocked.

## Basic troubleshooting steps

1. Check the traceroute output and identify the first hop where responses stop.
2. Check the route to the destination on the relevant router, if you have access:
   ~~~text
   show ip route 8.8.8.8
   ~~~
3. Check whether UDP probes or the returning ICMP messages may be filtered by an ACL or firewall.
4. Compare the result with ping and other available tests. A successful ping and a failed UDP traceroute can happen when the probe types are treated differently.
5. Repeat the test if the result may have been caused by temporary loss or delay.

## Important when interpreting results

- A timeout at one hop does not always mean traffic stops there. Some routers do not answer traceroute probes but still forward later packets.
- If later hops respond, the path is continuing beyond the hop that showed a timeout.
- A Type 11, Code 0 response identifies a hop where the probe's TTL expired.
- A Type 3, Code 3 response is the normal stopping response for many UDP traceroute implementations when the destination is reached and the selected UDP port is closed.
- Traceroute measures the RTT to each responding hop. It does not directly measure the one-way delay across an individual link.
