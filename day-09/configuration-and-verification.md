# DHCP Configuration and Verification Checklist

This checklist is a reference for documenting the DHCP lab. Replace example values with the exact commands and outputs from my own lab evidence.

## Example Cisco IOS DHCP server configuration

The following is an illustrative example for a `192.168.1.0/24` network, not a claim that these exact values were used in the lab:

```text
ip dhcp excluded-address 192.168.1.200 192.168.1.250
ip dhcp pool LAN-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8 1.1.1.1
```

The excluded range prevents the DHCP server from leasing those addresses from this pool. The default-router value should be the actual gateway for the client subnet.

## Useful verification commands

### On a Cisco IOS DHCP server/router

```text
show running-config | section dhcp
show ip dhcp pool
show ip dhcp binding
```

These help inspect the DHCP configuration, pool utilization, and active lease bindings.

### On a client

Use the operating system's IP configuration tools to check the assigned IPv4 address, subnet mask, default gateway, and DNS servers. Record the exact command and output for the lab platform used.

## Lab evidence checklist

- [ ] Record the topology and which link was captured.
- [ ] Record the DHCP pool, excluded addresses, gateway, and DNS settings from the actual configuration.
- [ ] Verify the client received an address and the expected options.
- [x] Capture and identify Discover, Offer, Request, and ACK on the R1–Switch1 link.
- [x] Add the R2–Switch capture and compare the visible Offer/ACK destination addresses.
- [ ] Confirm whether the client broadcast flag is set in the second capture (not visible in the supplied screenshot).
- [ ] Confirm the leased client address does not conflict with the intended gateway address.

## DHCP snooping

DHCP snooping is a switch security feature that can help prevent rogue DHCP servers from providing unauthorized leases. Ports facing legitimate DHCP servers or trusted relay paths are configured as trusted; untrusted access ports are restricted from sending server-side DHCP messages. I will only mark a DHCP snooping lab as completed if I have configuration and verification evidence for it.
