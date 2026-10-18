# Day 05 — Packet-Level Walkthrough: ARP, ICMP, and Routing

I use this page to connect the commands I ran with the headers and packets Wireshark shows. Case 1 is based on my same-subnet capture. For Case 2, I captured the PC3-to-R1 link and can see ICMP Echo Requests and Echo Replies between the two subnets. I have not captured every ARP step or the full packet flow on both router links.

## Case 1 — PC1 pings PC2 on the same subnet

### Topology

| Device | IPv4 address | MAC address shown in capture |
|---|---|---|
| PC1 | `192.168.1.1/24` | `00:50:79:66:68:00` |
| PC2 | `192.168.1.2/24` | `00:50:79:66:68:01` |

Both addresses belong to `192.168.1.0/24`. PC1 can send directly to PC2 on the local Ethernet LAN, so it does not need to send this traffic to a default gateway.

### Packet 1 — ARP request

In my Wireshark list, the first packet says: `Who has 192.168.1.2? Tell 192.168.1.1`.

| Field | What the capture shows | Meaning |
|---|---|---|
| Ethernet destination | `ff:ff:ff:ff:ff:ff` | Broadcast to devices in the local Layer 2 broadcast domain |
| Ethernet source | `00:50:79:66:68:00` | PC1's MAC address |
| Ethernet Type | `0x0806` | The Ethernet payload is ARP |
| ARP opcode | Request (1) | PC1 is asking for the MAC address associated with an IPv4 address |
| Sender protocol address | `192.168.1.1` | PC1's IPv4 address |
| Target protocol address | `192.168.1.2` | The IPv4 address PC1 wants to reach |
| Target hardware address | `ff:ff:ff:ff:ff:ff` in the displayed decode | The request is sent in a broadcast Ethernet frame; the target MAC is not yet known |

ARP is carried directly inside the Ethernet frame, not inside an IPv4 packet. That is why this packet has an ARP decode rather than an IPv4 header.

### What the switch does with the ARP request

1. The switch receives the frame from PC1.
2. It can learn PC1's source MAC address on the port where the frame arrived.
3. Because the destination is broadcast, it floods the frame out the other eligible ports in the same VLAN, except the incoming port.
4. PC2 receives the request and checks the target IPv4 address. It matches PC2's address, so PC2 can reply.

### Packet 2 — ARP reply

My packet list shows an ARP reply with the information `192.168.1.2 is at 00:50:79:66:68:01`.

- The sender protocol address is `192.168.1.2`.
- The sender hardware address is `00:50:79:66:68:01`, PC2's MAC.
- The reply is normally sent as a unicast Ethernet frame back to PC1.
- PC1 can store the IPv4-to-MAC mapping in its ARP cache. The switch can also learn PC2's source MAC address from the reply.

The purpose of ARP is to resolve the local IPv4 next hop to a MAC address. It does not test whether an application is available.

### Packet 3 — ICMP Echo Request

After ARP resolution, the capture shows an ICMP Echo request from `192.168.1.1` to `192.168.1.2`.

| Header/field | Value or interpretation |
|---|---|
| Ethernet source MAC | PC1: `00:50:79:66:68:00` |
| Ethernet destination MAC | PC2: `00:50:79:66:68:01` |
| IPv4 source | `192.168.1.1` |
| IPv4 destination | `192.168.1.2` |
| IPv4 Protocol field | `1` = ICMP |
| ICMP message | Echo Request, normally Type 8, Code 0 |

The Ethernet frame delivers the packet across the local link. The IPv4 addresses identify the Layer 3 endpoints, and ICMP carries the echo test. The IP protocol number `1` is not a TCP or UDP port number.

In the supplied expanded IPv4 decode, Wireshark shows IPv4, a 20-byte IPv4 header, TTL 64, source `192.168.1.1`, destination `192.168.1.2`, and `Protocol: ICMP (1)`. The capture's packet list shows a 98-byte frame for this ICMP message. Frame sizes can vary with the utility, payload, and capture environment.

### Packet 4 — ICMP Echo Reply

PC2 responds with an ICMP Echo Reply:

- Ethernet source is PC2's MAC and Ethernet destination is PC1's MAC.
- IPv4 source is `192.168.1.2` and destination is `192.168.1.1`.
- ICMP Echo Reply is normally Type 0, Code 0.
- The reply lets the ping utility report a round-trip time.

My terminal screenshot shows five replies from `192.168.1.2`. The Wireshark packet list also shows five Echo Requests paired with five Echo Replies.

### Why the ARP packets may not always appear

If the sender already has a valid ARP cache entry, it may send ICMP without first generating a new ARP request. A fresh lab, cleared cache, or first contact often makes the ARP exchange easier to observe. My supplied capture shows ARP before the ICMP sequence.

## Case 2 — PC3 pings PC4 through a router

### Addressing in my topology

| Device | IPv4 address | Role |
|---|---|---|
| PC3 | `192.168.1.1/24` | Source host |
| R1 G1/0 | `192.168.1.100/24` | PC3's default gateway |
| R1 G2/0 | `192.168.20.200/24` | PC4's default gateway |
| PC4 | `192.168.20.1/24` | Destination host |

PC3 and PC4 are in different subnets. I configured the router interfaces and default gateways, and I was able to ping PC4 from PC3. My capture on the PC3-to-R1 G1/0 link shows ICMP Echo Requests from PC3 to PC4 and Echo Replies in the reverse direction. The full ARP and forwarding sequence below explains the expected flow; the screenshots do not expose every step or both router links.

### Step 1 — PC3 determines the destination is remote

PC3 compares `192.168.20.1` with its own address and `/24` mask. The destination is not in `192.168.1.0/24`, so PC3 sends the packet toward its configured default gateway, `192.168.1.100`.

### Step 2 — PC3 resolves the gateway MAC

If the gateway mapping is not cached, PC3 broadcasts an ARP request for `192.168.1.100` on the PC3 LAN.

- Ethernet destination: broadcast `ff:ff:ff:ff:ff:ff`.
- ARP sender IP: `192.168.1.1`.
- ARP target IP: `192.168.1.100`.

PC3 does **not** ARP directly for PC4's MAC address, because PC4 is on a remote subnet. The router's G1/0 interface responds with its own MAC address.

### Step 3 — PC3 sends the packet to R1

The Ethernet frame on the PC3-to-R1 link has:

- Ethernet source MAC: PC3's MAC.
- Ethernet destination MAC: R1 G1/0's MAC.
- IPv4 source: `192.168.1.1`.
- IPv4 destination: `192.168.20.1`.

The key point is that the Ethernet destination is the local gateway, but the IP destination remains PC4. These are different header layers and serve different purposes.

### Step 4 — R1 routes the packet

R1 removes the incoming Ethernet header and checks the IP destination against its routing table. Because `192.168.20.0/24` is directly connected to G2/0, R1 forwards the packet out that interface.

A router creates a new Layer 2 frame for the outgoing link. Without NAT, the source and destination IP addresses ordinarily remain `192.168.1.1` and `192.168.20.1`; the IPv4 TTL is decremented and the IPv4 header checksum is updated. Ethernet source and destination MAC addresses are different on the outgoing link.

### Step 5 — R1 resolves PC4's MAC if needed

If R1 does not already have an ARP entry for `192.168.20.1`, it broadcasts an ARP request on the destination LAN. PC4 replies with its MAC address. R1 can then send the routed packet in a frame addressed to PC4.

### Step 6 — PC4 replies through its gateway

PC4 sees that `192.168.1.1` is outside its local `192.168.20.0/24` subnet, so it sends the reply toward its gateway, `192.168.20.200`. R1 routes the reply toward PC3 and uses a new Layer 2 frame on the PC3 LAN.

## Proxy ARP clarification

Resolving a default gateway's MAC address is ordinary ARP, not Proxy ARP. Proxy ARP is a distinct router behavior where a router answers an ARP request on behalf of another IP address under specific conditions. My topology works through normal routing with configured gateways; the screenshots provided so far do not establish that Proxy ARP is enabled or used.

## Troubleshooting using packet evidence

| Symptom | What I would check |
|---|---|
| No ARP reply for a local target or gateway | IP/mask, correct VLAN/link, interface state, target address, and whether ARP requests/replies are visible on the expected link |
| ARP works but no ICMP Echo Reply | IP routing, host settings, ICMP filtering, and return path |
| Request reaches remote LAN but no reply returns | PC4 address/mask/gateway, host filtering, and reverse routing |
| Ping succeeds but an application fails | Application service, TCP/UDP port, host firewall, and application configuration |
| Ping fails | Do not immediately conclude the host is down; ICMP can be filtered or rate-limited |

A missing ARP entry alone does not prove a physical-layer fault. I need to use multiple observations to narrow down the cause.

## What I have and have not captured

- **Captured for Case 1:** Topology, successful ping output, ARP request details, and an expanded IPv4 header showing ICMP protocol number 1.
- **Captured for Case 2:** Wireshark packet-list evidence on the PC3-to-R1 G1/0 link showing ICMP Echo Requests from `192.168.1.1` to `192.168.20.1` and Echo Replies in the reverse direction. The visible list also contains CDP and local LOOP packets.
- **Observed separately:** A capture with repeated gratuitous ARP announcements for `192.168.1.1`, `192.168.1.3`, and `192.168.10.1`. The screenshot alone does not establish an address conflict or the reason for each announcement.
- **Still not fully verified:** The complete gateway ARP exchange, every routed packet field, and the packet flow on the R1-to-PC4 link. I will label those as expected behavior unless I capture the relevant packets.
