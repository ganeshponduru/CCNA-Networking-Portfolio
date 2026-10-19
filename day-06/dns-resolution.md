# DNS Resolution — My Notes

## Why DNS is useful

I learned that people usually remember names more easily than IP addresses. DNS (Domain Name System) lets applications look up information associated with a domain name, including the IP address needed to connect to a service.

DNS is not limited to converting a name to an IP address. It stores different record types for different purposes, and the answer can vary depending on the record, resolver, location, and time.

## DNS records I want to remember

| Record | What I use it for |
|---|---|
| A | Maps a name to an IPv4 address |
| AAAA | Maps a name to an IPv6 address |
| CNAME | Makes one name an alias of another name |
| PTR | Used for reverse DNS lookup, mapping an IP address to a name |
| NS | Identifies name servers for a DNS zone |
| MX | Identifies mail servers for a domain |

An A record is for IPv4. An AAAA record is for IPv6.

## What happens when I open a website?

The exact sequence can vary because operating systems, browsers, and networks may have their own caches and configuration. This is the general flow I am learning:

1. I enter a domain name in the browser.
2. The browser or operating system checks applicable local caches and name-resolution sources. A hosts-file entry may also answer the name without a DNS query.
3. If an answer is not available locally, the device asks its configured DNS resolver, often a router, ISP resolver, or public recursive resolver.
4. The resolver checks its cache. If it does not have a usable answer, it performs the necessary DNS lookups.
5. For a typical uncached public-domain lookup, the resolver may ask a root server which TLD name servers to use, ask a TLD server for the domain's authoritative name servers, and then ask an authoritative server for the requested record.
6. The resolver returns the answer to my device and may cache it according to the record's TTL.
7. The browser can then use the returned address to try to connect to the destination service.

The client usually sends a **recursive query** to its configured resolver, asking it to return the final answer or an error. The resolver generally uses **iterative queries** to follow referrals through the DNS hierarchy. Caching can mean that some of these upstream steps are skipped.

## DNS packet details

- DNS commonly uses UDP port 53.
- DNS can also use TCP port 53, for example for zone transfers and when TCP is needed for a DNS response or exchange.
- A client query normally uses a temporary source port and destination port 53.
- The DNS message includes fields such as the query name, query type, flags, and response code.
- The **Recursion Desired (RD)** bit in a client query asks the resolver to perform recursion. It does not mean the client itself is contacting root, TLD, and authoritative servers.
- The reply may contain one or more answers. Multiple returned addresses may support resilience, geographic distribution, CDN behaviour, or other designs; the packet alone does not prove which one is being used.

## Root servers: the clarification I made

There are 13 named root-server identities, traditionally labelled A through M. They are not merely 13 physical machines located in 13 places. Each identity is served by many instances around the world, and anycast routing can direct queries to an appropriate instance. A resolver does not simply select a single “nearest physical root server” and use it for every lookup.

Root servers generally refer resolvers to the relevant TLD name servers; they do not hold every website's final IP address.

## How I can inspect DNS in Wireshark

1. Start a capture on the network interface I am actually using.
2. Generate a lookup for a domain that is not already answered from cache, if possible.
3. Stop the capture and apply the display filter dns.
4. Find a DNS query and its corresponding response.
5. Expand the DNS section and inspect the query name, query type, flags, response code, and answer records.
6. Inspect the IP and UDP/TCP headers to identify the client, resolver, ports, and transport.
7. If no DNS packet appears, I should not immediately assume DNS is broken. The answer may have come from a cache or hosts file, I may have captured on the wrong interface, or encrypted DNS such as DoH/DoT may be in use.

Useful Windows commands:

    ipconfig /all
    ipconfig /displaydns
    ipconfig /flushdns
    nslookup example.com
    nslookup -type=AAAA example.com

ipconfig /displaydns displays the Windows DNS resolver cache; it is not a packet-capture command. ipconfig /flushdns clears the Windows DNS resolver cache, so I should use it only when I intend to clear that cache for a test.

## Troubleshooting notes

If a website name does not resolve, I would check step by step:

- Does the device have valid IP addressing and a working route to its resolver?
- Which DNS server is configured?
- Can I query the resolver with nslookup?
- Does the query time out, return an error, or return an unexpected record?
- Is the issue limited to one name, or do multiple names fail?
- Could the result be cached, affected by a hosts-file entry, or returned differently by another resolver?

A successful ping to an IP address does not prove that DNS is working. Conversely, a failed ping to a domain does not by itself prove DNS is broken; ICMP may be blocked while DNS and the application still work.
