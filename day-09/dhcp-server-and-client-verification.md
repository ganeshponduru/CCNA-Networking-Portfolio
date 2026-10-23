# DHCP Server Configuration and Client Verification

## Topology shown

The screenshot shows R1 configured as the DHCP server, connected through Switch1 to PC1 and PC2. A second topology image supplied for this day also shows PC3, PC4 and R2, but this configuration/output screenshot verifies leases for PC1 and PC2 only.

## Observed R1 configuration

The visible running-config section shows:

```text
ip dhcp pool SBI-BANK
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.100
 dns-server 192.168.1.150 192.168.1.151
ip dhcp excluded-address 192.168.1.200 192.168.1.250
interface GigabitEthernet1/0
 ip address 192.168.1.100 255.255.255.0
 no shutdown
```

The screenshot shows the interface and line protocol changing to up after `no shutdown`.

### Why exclude an address range?

The configured range `192.168.1.200–192.168.1.250` is excluded from DHCP allocation. In this setup, the router interface/gateway address `192.168.1.100` is outside that excluded range; the exclusion itself does not reserve the gateway, but prevents DHCP from allocating the specified addresses.

## Client verification

The VPCS output shows successful `ip dhcp` results and `show ip` output:

| Field | PC1 | PC2 |
|---|---|---|
| IPv4 address/mask | `192.168.1.1/24` | `192.168.1.2/24` |
| Gateway | `192.168.1.100` | `192.168.1.100` |
| DNS server 1 | `192.168.1.150` | `192.168.1.150` |
| DNS server 2 | `192.168.1.151` | `192.168.1.151` |
| DHCP server | `192.168.1.100` | `192.168.1.100` |

The displayed lease information is `86397, 86400/43200/75600`. The screenshot does not label every number separately, so avoid assigning a specific meaning to each number beyond what the VPCS output itself indicates.

## What this proves

- The DHCP pool is configured for `192.168.1.0/24`.
- The router interface uses `192.168.1.100/24` and was brought up.
- PC1 and PC2 both successfully requested and received IPv4 configuration.
- Both clients show the intended gateway and DNS servers.

This screenshot does not show the output of `show ip dhcp binding` or `show ip dhcp pool`; those commands are useful additional verification, but should not be described as executed unless their output is captured.

## Correlating the configuration screenshot with the packet capture

The DORA packet screenshots show an offered address of `192.168.1.5` and client hardware address `ca:02:1f:ec:00:1c`. This client-verification screenshot shows PC1 at `192.168.1.1` with MAC `00:50:79:66:68:00`, and PC2 at `192.168.1.2` with MAC `00:50:79:66:68:01`.

Those client identifiers do not match. Therefore, the DORA capture and this VPCS output should be documented as separate pieces of evidence unless the capture can be tied to the same client/session. Do not claim that the DORA packets specifically assigned `192.168.1.1` or `.2`; the visible Offer/ACK instead show `192.168.1.5`.
