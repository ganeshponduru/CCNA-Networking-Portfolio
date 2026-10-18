# Common Protocols and Port Numbers

I use this table as a quick revision sheet for common services, their default ports, and the transport protocols they use.
Port numbers identify application/service endpoints at the transport layer. These are common defaults; deployments can use other ports.

| Protocol / service | Common port(s) | Transport | Purpose / note |
|---|---:|---|---|
| FTP control | 21 | TCP | Control connection |
| FTP data (traditional active mode) | 20 | TCP | Traditional server-side data port in active mode; not every transfer uses it |
| SSH | 22 | TCP | Secure remote login and command execution |
| Telnet | 23 | TCP | Remote terminal; no built-in encryption |
| SMTP | 25 | TCP | Mail transfer between servers; submission commonly uses other ports |
| DNS | 53 | UDP/TCP | Name resolution |
| DHCP server/client | 67 / 68 | UDP | IPv4 address configuration |
| HTTP | 80 | TCP | Common default for HTTP |
| NTP | 123 | UDP | Time synchronization |
| SNMP | 161 / 162 | UDP | Queries and notifications/traps |
| LDAP | 389 | TCP/UDP | Directory access; common deployments use TCP |
| HTTPS | 443 | TCP; UDP for QUIC/HTTP/3 | Encrypted web traffic; transport depends on HTTP version |

## Remember

- A port number is not the same as an IP protocol number.
- SSH commonly uses TCP 22; Telnet commonly uses TCP 23.
- DNS can use UDP and TCP.
- HTTP/3 uses QUIC over UDP, commonly port 443.
- A service may use a non-default port; verify actual configuration when troubleshooting.

For authoritative assignments, consult the IANA registry linked in references.md.