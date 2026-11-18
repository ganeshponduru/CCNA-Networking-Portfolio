# Reading Traceroute Output and Troubleshooting

## Reading the output

A traceroute result normally displays one line for each TTL value (hop). The line contains the hop number, the responding router address or name, and the RTT measurements for the probes.

In the packet capture from this lab, the responses were:

- TTL 1 — R2, 192.168.12.2
- TTL 2 — R3, 192.168.23.3
- TTL 3 — R4 response source, 192.168.34.4

The address shown is the source address used in the ICMP response. It is not necessarily a router ID or loopback address. In this capture, the final probe destination was 4.4.4.4, while the Port Unreachable response came from 192.168.34.4.

## What does * * * mean?

An asterisk means that traceroute did not receive a response for that probe before its timeout. It does **not** prove by itself that the router is down or that there is no route.

Possible reasons include:

1. The router has no route to the destination.
2. UDP probes or ICMP replies are filtered by an ACL or firewall.
3. The router does not send, or rate-limits, the ICMP response.
4. A probe or response was lost or delayed.
5. The forward or return path has a problem.

## Basic troubleshooting steps

1. Check the traceroute output and identify the first hop where responses stop.
2. Check the route to the destination on the relevant router:
   ~~~text
   show ip route 4.4.4.4
   ~~~
3. Check whether UDP probes or the returning ICMP messages may be filtered by an ACL or firewall.
4. Compare the result with ping and other available tests. A successful ping and a failed UDP traceroute can happen when the probe types are treated differently.
5. Repeat the test if the result may have been caused by temporary loss or delay.

## Important when interpreting results

- A timeout at one hop does not always mean traffic stops there. Some routers do not answer traceroute probes but still forward later packets.
- If later hops respond, the path is continuing beyond the hop that showed a timeout.
- A Type 11, Code 0 response identifies a hop where the probe's TTL expired.
- A Type 3, Code 3 response means a Port Unreachable message was returned for the UDP probe.
- Traceroute measures the RTT to each responding hop. It does not directly measure the one-way delay across an individual link.
