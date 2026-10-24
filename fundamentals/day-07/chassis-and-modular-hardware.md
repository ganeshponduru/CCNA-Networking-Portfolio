# Chassis, Line Cards, and Redundancy

I'm learning how larger enterprise and service-provider routers are built differently from fixed-port switches.

## Fixed-port and modular devices

A fixed-port device has most of its interfaces built into the chassis. A modular chassis accepts replaceable cards or modules, allowing the platform to be configured for the required interface types and capacity.

## Line cards / I/O modules

A line card or I/O module provides interfaces for connecting to other network devices. The supported interface speed and type depend on the particular card and chassis.

An empty slot may use a blanking panel. These panels help maintain the airflow design and protect the opening; I should follow the platform's installation guide.

## Control-plane and routing modules

Some chassis platforms separate interface cards from control-plane/system-management modules. Vendor terms differ:
- Cisco platforms may use terms such as **Route Switch Processor (RSP)** or **I/O module (IOM)**, depending on the product family.
- Juniper platforms may use **Routing Engine** for control-plane functions.

I should not assume that every router has an RSP or that the operating system is always installed on a particular removable module. The architecture is platform-specific.

## Redundancy

Some devices provide redundant PSUs or control modules. A common design uses an active module and a standby module, with platform-specific mechanisms for switchover. Redundancy can reduce downtime, but it does not guarantee uninterrupted service in every failure scenario.

Before replacing a module, I should:
1. Confirm the fault using alarms, logs, status indicators, and supported diagnostic commands.
2. Identify the exact chassis, slot, module type, and part number.
3. Check whether the component is hot-swappable and whether removing it affects traffic.
4. Follow the vendor procedure and the site's change-control process.
5. Record serial/part details and engage vendor support if required.

## Interface naming

On modular devices, an interface name can encode hardware location such as slot, module, and port, but the exact format differs by vendor and operating system. I should read the device's own interface descriptions and hardware guide rather than applying one vendor's naming pattern to another.

## Useful Cisco show commands from my notes

These are examples to recognize; command availability and output can vary by platform and software:

```text
show version
show running-config
show startup-config
show ip interface brief
show interfaces status
show ip route
show vlan brief
show etherchannel summary
show cdp neighbors
show lldp neighbors
```

Some commands in a handout are feature-specific or platform-specific. For example, commands that inspect DHCP, OSPF, EIGRP, VRRP, or spanning tree depend on whether that feature is configured and supported.

## What I should be able to explain

- Why a modular router uses line cards.
- Why control-plane modules may be redundant.
- Why hardware names differ between vendors.
- Why I should check platform documentation before inserting or removing a module.
