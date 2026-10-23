# Day 9 — DHCP Fundamentals and DORA Packet Analysis

## What I covered

- DHCP purpose and client/server roles
- DHCP Discover, Offer, Request and Acknowledgment (DORA)
- IPv4 broadcast addressing and UDP ports 67/68
- DHCP options used to deliver client network settings
- Reading DHCP packet fields in Wireshark
- Basic Cisco IOS DHCP verification commands

## Lab topology

The screenshots show R1 labelled as the DHCP server connected to Switch1, with VPCS clients. A second topology screenshot also includes PC3, PC4 and R2. The packet capture shows one client's DORA exchange.

## Evidence and analysis

- [DHCP DORA packet walkthrough](./dora-packet-walkthrough.md)
- [DHCP options and header fields](./dhcp-options-and-fields.md)
- [DHCP server configuration and client verification](./dhcp-server-and-client-verification.md)
- [Configuration and verification checklist](./configuration-and-verification.md)

## Lab evidence

The terminal screenshot documents the R1 DHCP pool and successful client lease output for PC1 and PC2. The packet screenshots document a DHCP Discover, Offer, Request and ACK using transaction ID `0x00002242`. The client MAC shown in the DHCP messages is `ca:02:1f:ec:00:1c`; the offered and acknowledged address is `192.168.1.5`; the DHCP server identifier is `192.168.1.100`.

The packet capture and client-output screenshot show different client MAC addresses and different assigned IPs, so they are recorded as distinct evidence rather than incorrectly treating them as the same DHCP transaction. Screenshot files still need to be uploaded to the repository before they can be embedded in the Markdown.


## Screenshot evidence

The images below are the screenshots uploaded to this folder. They document the lab topology, packet capture and DHCP configuration/client output.

### DHCP lab topology

![DHCP lab topology](./Screenshot%202026-10-23%20081913.png)

### Wireshark DHCP packet list showing DORA

![Wireshark DHCP packet list showing DORA](./Screenshot%202026-10-23%20081959.png)

### DHCP Discover packet fields

![DHCP Discover packet fields](./Screenshot%202026-10-23%20085505.png)

### DHCP Offer packet fields

![DHCP Offer packet fields](./Screenshot%202026-10-23%20090058.png)

### DHCP Request packet fields

![DHCP Request packet fields](./Screenshot%202026-10-23%20090214.png)

### DHCP ACK packet fields

![DHCP ACK packet fields](./Screenshot%202026-10-23%20090444.png)

### Additional DHCP packet analysis

![Additional DHCP packet analysis](./Screenshot%202026-10-23%20090542.png)

### DHCP server configuration and VPCS client verification

![DHCP server configuration and VPCS client verification](./Screenshot%202026-10-23%20091407.png)

## Scope

This is a lab-based packet-analysis note. The screenshots establish the DORA exchange for the displayed client; they do not establish that every client in the larger topology obtained a lease, nor do they demonstrate DHCP Snooping configuration.

## Key takeaway

DHCP allows a client to obtain an IP configuration dynamically. In this capture, the client starts with source IP `0.0.0.0`, broadcasts its Discover to `255.255.255.255`, and uses UDP source port 68 to destination port 67. The server's Offer and ACK advertise the offered address and network parameters.
