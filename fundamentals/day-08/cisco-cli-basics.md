# Cisco IOS CLI Modes and Interface Basics

## CLI modes

| Mode | Typical prompt | Purpose |
|---|---|---|
| User EXEC | `R1>` | Limited operational commands |
| Privileged EXEC | `R1#` | More detailed verification and management commands |
| Global configuration | `R1(config)#` | Configure device-wide settings |
| Interface configuration | `R1(config-if)#` | Configure an individual interface |
| VTY line configuration | `R1(config-line)#` | Configure remote terminal access settings |

The prompt can vary according to hostname and configuration. Privileged EXEC is entered with `enable`; global configuration is entered with `configure terminal`. From configuration mode, `end` returns to privileged EXEC. On IOS, `do <EXEC-command>` can run many EXEC commands without leaving configuration mode.

## Interface addressing example

In an isolated lab, a basic interface configuration may look like:

```text
enable
configure terminal
interface GigabitEthernet1/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
end
show ip interface brief
```

The interface name depends on the device model and available interfaces. `no shutdown` removes an administrative shutdown; it does not guarantee that the physical/link protocol will become up. The interface also needs a working peer/link and compatible configuration.

## Useful verification commands

- `show ip interface brief` — quick view of interface IP addresses and status.
- `show version` — platform/software and device information.
- `show inventory` — hardware inventory on platforms that support it.
- `show ip ssh` — SSH server status and settings on supported IOS versions.
- `show arp` — ARP table on many Cisco IOS devices.
- `ping <destination-ip>` — tests IP reachability, subject to routing and filtering.

## ARP and first-ping behavior

On a directly connected IPv4 Ethernet subnet, a router may need to resolve the peer's MAC address with ARP before it can send the first ICMP Echo Request. This can contribute to an initial ping timeout in some circumstances, but it is not the only possible cause. Confirm with ARP and packet evidence rather than assuming every first-packet loss is due to ARP.
