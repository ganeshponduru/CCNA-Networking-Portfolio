# IPv4 Addressing, Classes and Private Ranges

I use this page to revise IPv4 address structure, private ranges, and the difference between historical classful addressing and modern CIDR.
## IPv4 basics
IPv4 addresses are 32 bits long, represented as four 8-bit octets in dotted-decimal notation. Each octet ranges from 0 to 255 because 2^8 = 256 possible values, numbered 0 through 255.

Example: 192.168.1.10 is represented in binary as 11000000.10101000.00000001.00001010.

## Legacy classful ranges

| Class | First-octet range | Historical default mask | Original role |
|---|---|---|---|
| A | 0–127 (ordinary unicast historically 1–126) | 255.0.0.0 (/8) | Large networks |
| B | 128–191 | 255.255.0.0 (/16) | Medium networks |
| C | 192–223 | 255.255.255.0 (/24) | Smaller networks |
| D | 224–239 | Not a host-unicast class | IPv4 multicast |
| E | 240–255 | Not ordinary host unicast | Reserved/experimental space |

**Modern networks use CIDR (Classless Inter-Domain Routing), not old classful defaults, for address allocation and routing.** A prefix such as /26 specifies how many leading bits identify the network. In modern configurations, read the actual subnet mask/prefix rather than inferring it from the address class.

Special cases:
- 0.0.0.0/8 has special meaning and is not ordinary public host space.
- 127.0.0.0/8 is loopback; 127.0.0.1 commonly refers to the local host.
- 255.255.255.255 is the limited broadcast address.
- Class D is multicast, not a normal unicast host-address pool.

## RFC 1918 private IPv4 ranges

| Private range | CIDR | Historical association |
|---|---|---|
| 10.0.0.0–10.255.255.255 | 10.0.0.0/8 | Class A-sized range |
| 172.16.0.0–172.31.255.255 | 172.16.0.0/12 | Part of Class B range |
| 192.168.0.0–192.168.255.255 | 192.168.0.0/16 | Class C-sized ranges |

Do not treat all 172.x.x.x addresses as private: only 172.16.0.0 through 172.31.255.255 are in the RFC 1918 range. Likewise, the private 192 block is 192.168.0.0/16, not all 192.x.x.x.

## Private/public addressing and NAT
Private addresses are intended for private networks and are not globally routed on the public Internet by default. NAT commonly lets private-addressed hosts share public IPv4 connectivity, but not every Internet-connected device must use a private address. Public addresses are allocated through the Internet number registry system and service providers; a domain name does not mean a server always has one fixed public IP.

## Subnet masks and prefixes
A subnet mask marks network-prefix bits with ones and host bits with zeros:
- /8 = 255.0.0.0
- /16 = 255.255.0.0
- /24 = 255.255.255.0

The number of bits left for host identifiers is 32 minus the prefix length. The total address patterns are 2^(host bits). Traditional subnets reserve network and directed-broadcast addresses, so usable host count is often total minus two; /31 point-to-point links and /32 host routes are special cases.

## Windows command
Use ipconfig /all in Windows Command Prompt to inspect IPv4 address, subnet mask, default gateway, and DNS servers. Do not publish your real public IP, MAC address, or sensitive network details in a public repository.