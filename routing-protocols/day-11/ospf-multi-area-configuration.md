# OSPF Configuration and Multi-Area Design

## OSPF configuration mode

Basic Cisco IOS syntax:

```text
Router(config)# router ospf <process-id>
Router(config-router)# network <network-address> <wildcard-mask> area <area-id>
```

Example:

```text
Router(config)# router ospf 10
Router(config-router)# network 192.168.13.0 0.0.0.255 area 0
```

The process ID identifies the OSPF process locally on the router. It is not an autonomous system number and does not need to match on neighboring routers.

The OSPF `network` command uses a wildcard mask to select local interfaces whose IP addresses match the statement. It enables OSPF on those interfaces and places them in the specified area. The command does not advertise an arbitrary remote network simply because its network address is typed.

## Wildcard masks

A wildcard mask is the inverse of a subnet mask.

| Subnet mask | Wildcard mask | Meaning |
|---|---|---|
| 255.255.255.255 | 0.0.0.0 | Match one exact interface IP address |
| 255.255.255.0 | 0.0.0.255 | Match the first 24 bits |
| 255.255.0.0 | 0.0.255.255 | Match the first 16 bits |
| 0.0.0.0 | 255.255.255.255 | Match any IPv4 address |

Calculation:

```text
Wildcard mask = 255.255.255.255 - subnet mask
```

For a /24 network:

```text
Subnet mask:   255.255.255.0
Wildcard mask:   0.0.0.255
```

For a /32 loopback address, use `0.0.0.0` to match that exact address.

## OSPF areas

OSPF can divide a routing domain into areas to limit the scope of link-state information and improve scalability.

- **Area 0:** The backbone area.
- **Non-backbone area:** An additional OSPF area connected to the backbone through an ABR.
- **ABR (Area Border Router):** A router with interfaces in multiple OSPF areas. It maintains area-specific link-state information and advertises inter-area reachability.
- **Intra-area route:** A route learned within the same OSPF area.
- **Inter-area route:** A route learned from another OSPF area.

For a standard multi-area design, non-backbone areas should connect to Area 0. The OSPF process ID is locally significant; the area assignment on the connected interfaces is what must be compatible for adjacency formation.

## Multi-area lab addressing reference

The following addressing plan represents the five-router topology used for the configuration walkthrough.

| Router | Loopback | Physical links | OSPF area |
|---|---|---|---|
| R1 | 1.1.1.1/32 | 192.168.13.1/24 to R3; 192.168.14.1/24 to R4 | Area 0 |
| R2 | 2.2.2.2/32 | 192.168.23.2/24 to R3; 192.168.24.2/24 to R4; 192.168.25.2/24 to R5 | Area 0 on links to R3/R4; Area 10 on link to R5 |
| R3 | 3.3.3.3/32 | 192.168.13.3/24 to R1; 192.168.23.3/24 to R2 | Area 0 |
| R4 | 4.4.4.4/32 | 192.168.14.4/24 to R1; 192.168.24.4/24 to R2 | Area 0 |
| R5 | 5.5.5.5/32 | 192.168.25.5/24 to R2 | Area 10 |

R2 connects Area 0 and Area 10, so R2 is the ABR in this topology.

## Example OSPF configuration

The following examples use process ID 10 on each router for consistency in the configuration walkthrough. The process ID could be different on another router without preventing OSPF adjacency, provided the interface-level OSPF parameters match.

### R1 — Area 0

```text
interface loopback0
 ip address 1.1.1.1 255.255.255.255

interface g1/0
 ip address 192.168.13.1 255.255.255.0
 no shutdown

interface g1/1
 ip address 192.168.14.1 255.255.255.0
 no shutdown

router ospf 10
 network 1.1.1.1 0.0.0.0 area 0
 network 192.168.13.0 0.0.0.255 area 0
 network 192.168.14.0 0.0.0.255 area 0
```

### R2 — Area 0 and Area 10 (ABR)

```text
interface loopback0
 ip address 2.2.2.2 255.255.255.255

interface g1/0
 ip address 192.168.23.2 255.255.255.0
 no shutdown

interface g2/0
 ip address 192.168.24.2 255.255.255.0
 no shutdown

interface e3/4
 ip address 192.168.25.2 255.255.255.0
 no shutdown

router ospf 10
 network 2.2.2.2 0.0.0.0 area 0
 network 192.168.23.0 0.0.0.255 area 0
 network 192.168.24.0 0.0.0.255 area 0
 network 192.168.25.0 0.0.0.255 area 10
```

### R3 — Area 0

```text
interface loopback0
 ip address 3.3.3.3 255.255.255.255

interface g1/0
 ip address 192.168.13.3 255.255.255.0
 no shutdown

interface g2/0
 ip address 192.168.23.3 255.255.255.0
 no shutdown

router ospf 10
 network 3.3.3.3 0.0.0.0 area 0
 network 192.168.13.0 0.0.0.255 area 0
 network 192.168.23.0 0.0.0.255 area 0
```

### R4 — Area 0

```text
interface loopback0
 ip address 4.4.4.4 255.255.255.255

interface g1/0
 ip address 192.168.14.4 255.255.255.0
 no shutdown

interface g2/0
 ip address 192.168.24.4 255.255.255.0
 no shutdown

router ospf 10
 network 4.4.4.4 0.0.0.0 area 0
 network 192.168.14.0 0.0.0.255 area 0
 network 192.168.24.0 0.0.0.255 area 0
```

### R5 — Area 10

```text
interface loopback0
 ip address 5.5.5.5 255.255.255.255

interface e3/0
 ip address 192.168.25.5 255.255.255.0
 no shutdown

router ospf 10
 network 5.5.5.5 0.0.0.0 area 10
 network 192.168.25.0 0.0.0.255 area 10
```

**Interface names must be checked against the actual router model and topology before pasting the configuration.** The IP addressing and area assignments are the important parts of the example; interface numbering can differ.

## Verification

Run these commands after configuration:

```text
show ip interface brief
show running-config | section router ospf
show ip ospf neighbor
show ip ospf database
show ip route
show ip protocols
ping 5.5.5.5
traceroute 5.5.5.5
```

Expected checks:

- Physical interfaces should be up/up.
- OSPF neighbors should reach the FULL state where an adjacency is expected.
- R2 should form adjacencies with routers on its Area 0 and Area 10 links.
- R1 should learn remote OSPF routes, including the 5.5.5.5/32 loopback in Area 10.
- OSPF-learned routes are marked with `O` in the IPv4 routing table. Inter-area routes are commonly shown as `O IA`.
- The default administrative distance for OSPF is 110.

## Troubleshooting checklist

1. Check interface status and IP addressing with `show ip interface brief`.
2. Confirm that both ends of a link are in the same IP subnet.
3. Confirm OSPF is enabled on the intended interfaces.
4. Confirm the area IDs match on the shared link.
5. Check the OSPF neighbor state with `show ip ospf neighbor`.
6. Inspect the OSPF configuration with `show running-config | section router ospf`.
7. Check the routing table and OSPF database.
8. Verify that the area design connects non-backbone areas to Area 0.
