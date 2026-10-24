# DHCP Configuration and Verification Commands

## Example Cisco IOS DHCP server configuration

This is an example for a `192.168.1.0/24` network. Use the actual addresses from the lab when documenting a specific configuration.

```text
ip dhcp excluded-address 192.168.1.200 192.168.1.250
ip dhcp pool LAN-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8 1.1.1.1
```

The excluded range prevents the DHCP server from leasing those addresses from this pool. The default-router value should match the actual gateway for the client subnet.

## Useful verification commands

On a Cisco IOS DHCP server/router:

```text
show running-config | section dhcp
show ip dhcp pool
show ip dhcp binding
```

These commands show the DHCP configuration, pool utilization, and active lease bindings.

On a client, check the assigned IPv4 address, subnet mask, default gateway, and DNS servers using the available IP configuration tools.

## DHCP snooping

DHCP snooping is a switch security feature that helps prevent rogue DHCP servers from offering unauthorized leases. Ports facing legitimate DHCP servers or trusted relay paths are configured as trusted; untrusted access ports are restricted from sending server-side DHCP messages.