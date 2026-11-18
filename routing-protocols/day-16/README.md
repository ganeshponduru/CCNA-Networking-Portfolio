# Traceroute and Tracert

In this topic, I learned how traceroute finds the routers between a source and a destination. The important part is how it uses the TTL field in the IP header and reads the ICMP messages returned by routers.

The notes cover the TTL process, the UDP probes used by the usual Linux/Unix traceroute command, the ICMP probes used by Windows tracert, and what to look for in a packet capture. Practical screenshots and the exact lab observations will be added after I complete the lab.

## Topics

| Topic | Notes |
|---|---|
| TTL and hop discovery | [TTL and Hop Discovery](ttl-and-hop-discovery.md) |
| Probe packets and ICMP responses | [Traceroute Packet Analysis](traceroute-packet-analysis.md) |
| Reading output and troubleshooting | [Troubleshooting](troubleshooting.md) |
| Revision | [Practice and Interview Questions](practice-questions.md) |

## Commands covered

**Cisco IOS**
~~~text
R5# traceroute 8.8.8.8
R1> traceroute 4.4.4.4 source 1.1.1.1
~~~

**Linux / Unix**
~~~bash
traceroute 8.8.8.8
~~~

**Windows**
~~~text
tracert 8.8.8.8
~~~

## Practical work

The practical section is not filled in yet. I will add the topology, command output, and Wireshark screenshots after running the lab. The screenshots will be kept in the [screenshots](screenshots/) folder.
