# ICMP and Ping

## What I learned

Ping is a command-line utility that commonly uses ICMP Echo Request and Echo Reply messages to test IP-layer reachability and measure round-trip time (RTT). It can also report packet loss across the test. A ping result does not prove that a particular application port or service is available.

## ICMP fields to recognise

| Message | ICMP Type | Code |
|---|---:|---:|
| Echo Reply | 0 | 0 |
| Echo Request | 8 | 0 |

ICMP is identified by IP protocol number `1`. For comparison, TCP uses IP protocol number `6` and UDP uses protocol number `17`. These are IP protocol numbers, not TCP/UDP port numbers.

## What I saw in Wireshark

The packet list showed ICMP Echo requests from `192.168.1.1` to `192.168.1.2`, followed by Echo replies from `192.168.1.2` to `192.168.1.1`. The expanded IPv4 header showed the source and destination addresses and `Protocol: ICMP (1)`.

The terminal displayed five replies. The reported times varied between packets, which is normal for measured RTTs in a virtual lab. I should not use a single universal latency threshold to decide whether a connection is good; acceptable latency depends on the destination and network path.

## Interpreting failures carefully

A failed ping can indicate a connectivity problem, but it does not always mean the destination host is powered off. ICMP may be blocked, filtered, or rate-limited, and the route or local addressing may be incorrect. I would check other evidence before deciding on the cause.
