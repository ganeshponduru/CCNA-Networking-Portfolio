# DHCP Configuration and Verification Checklist

This checklist is a reusable reference. The screenshots supplied for this Day 9 entry show DHCP packet exchange; they do not by themselves prove that every command below was run in this lab.

## Example Cisco IOS configuration

Use this as a study example only. Adjust the interface, subnet, exclusions and DNS addresses to match the actual lab.

```text
configure terminal
ip dhcp excluded-address 192.168.1.1 192.168.1.20
ip dhcp pool LAN-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8 1.1.1.1
exit
interface GigabitEthernet1/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
end
```

**Important:** do not copy this configuration blindly. In the packet capture, the DHCP server identifier/source is `192.168.1.100` and the offered address is `192.168.1.5`. Those observed values do not establish the exact running configuration or interface address. Use the configuration actually present in your lab.

## Verification commands

On a Cisco IOS DHCP server/router:

```text
show running-config | section dhcp
show ip dhcp pool
show ip dhcp binding
show ip interface brief
```

On VPCS, the common commands are:

```text
ip dhcp
show ip
```

Confirm that the client receives an address, subnet mask, default gateway and DNS configuration appropriate to the intended network. A successful DHCP exchange should be checked alongside the client’s resulting configuration.

## Packet capture filters

- `dhcp` — DHCP packets in Wireshark versions that use this display-filter name.
- `bootp` — commonly works for DHCP/BOOTP packet decoding in Wireshark.
- `udp.port == 67 || udp.port == 68` — filter by the standard DHCP UDP ports.

Display-filter names can vary by Wireshark version; use autocomplete if a filter is not recognized.

## Troubleshooting sequence

1. Check the client is configured to request an address dynamically.
2. Confirm physical/virtual links and the relevant VLAN are up.
3. Verify the DHCP pool network matches the client subnet.
4. Ensure excluded addresses do not consume the intended pool range.
5. Check the router interface or SVI has the correct address and is up.
6. If the server is on another subnet, verify DHCP relay configuration (for Cisco IOS, commonly `ip helper-address` on the client-facing Layer 3 interface).
7. Check the pool and active bindings, then capture the Discover/Offer/Request/ACK sequence.
8. Check for competing/rogue DHCP servers if clients receive unexpected settings.

## DHCP Snooping — theory only for this entry

DHCP Snooping is a Layer 2 switch security feature that can filter DHCP messages from untrusted ports and help prevent rogue DHCP server replies. Typically, client-facing ports remain untrusted and only the legitimate DHCP-server/relay-facing path is trusted. Exact commands and behavior depend on switch platform and software.

No DHCP Snooping lab is claimed in this portfolio entry unless separate configuration/output evidence is added.
