# Static and Default Routing — Interview and Practice Questions

This page covers static routing, default routing, administrative distance, route selection, floating static routes, and practical routing scenarios.

## Part 1 — Core interview questions

### 1. Explain the purpose of routing protocols. What are they?

**Answer:**

Routing protocols allow routers to learn and exchange information about reachable networks and determine paths for forwarding packets. They help routers share routing information so different networks can communicate.

Examples:
- **RIP:** distance-vector interior gateway routing protocol.
- **EIGRP:** advanced distance-vector interior gateway routing protocol.
- **OSPF:** link-state interior gateway routing protocol.
- **IS-IS:** link-state interior gateway routing protocol.
- **BGP:** path-vector exterior gateway routing protocol, used to exchange routes between autonomous systems.

A routing protocol is different from a routed protocol. IP carries packets; routing protocols help routers learn paths for IP networks.

### 2. What is Administrative Distance (AD)?

**Answer:**

Administrative Distance indicates how trustworthy a route source is considered by a router. When the same destination prefix is learned from different sources, the router generally prefers the route with the lower AD.

Common Cisco default administrative distances:

| Route source | Default AD |
|---|---:|
| Directly connected | 0 |
| Static route | 1 |
| eBGP | 20 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP (external) | 170 |
| iBGP | 200 |

**Example:** If the same 192.168.10.0/24 prefix is available through a static route with AD 1 and OSPF with AD 110, the static route is preferred.

AD compares route sources for the same prefix. It is not the same as a routing protocol metric.

Cisco's default administrative distance for IS-IS is 115.

### 3. How does the route-selection process work in a routing table?

**Answer:**

1. **Longest prefix match:** The most specific matching network prefix is selected.
2. **Administrative distance:** If competing routes have the same prefix length, the route source with the lower AD is preferred.
3. **Metric:** A routing protocol uses its metric to select the best path among routes learned through that protocol.

**Example 1 — Longest prefix match:**
- OSPF route: 10.10.10.10/32, AD 110.
- Static route: 10.10.10.0/24, AD 1.

The /32 route is more specific than /24. Traffic for 10.10.10.10 uses the /32 route, even though its AD is higher.

**Example 2 — Administrative distance:**
- OSPF route: 10.10.10.0/24, AD 110.
- Static route: 10.10.10.0/24, AD 1.

Both routes have the same prefix length. The static route is preferred because its AD is lower.

**Remember:** Longest prefix match comes before AD.

### 4. What is a default route?

**Answer:**

A default route is used when a router does not have a more-specific matching route for a destination. It provides a next hop for traffic whose destination is not otherwise present in the routing table.

The IPv4 default route is 0.0.0.0 0.0.0.0, also written as 0.0.0.0/0.

**Configuration example:**

~~~text
Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
~~~

This forwards traffic that has no more-specific route to next hop 10.0.0.1.

Default routes are commonly configured on office or branch routers to forward unknown destinations towards an upstream router or ISP. The gateway of last resort is the next hop used for traffic that matches the installed default route.

### 5. What is static routing? Explain its advantages and disadvantages.

**Answer:**

Static routing is a method in which an administrator manually configures routes on a router. The administrator specifies the destination network, subnet mask, and next-hop address or exit interface.

**Syntax:**

~~~text
Router(config)# ip route <destination-network> <subnet-mask> <next-hop-ip>
~~~

**Advantages:**
- Simple to use in small and stable networks.
- No routing-protocol update traffic is exchanged.
- Requires less CPU and memory for routing-protocol processing.
- The administrator controls the path.
- Useful for stub networks and specific routing requirements.

**Disadvantages:**
- Routes must be configured and maintained manually.
- Requires knowledge of the network topology.
- Becomes difficult to manage as the network grows.
- Does not automatically learn new routes or topology changes.
- A backup path must be configured deliberately; ordinary static routing does not provide dynamic path discovery.

Static routing is suitable for small networks or specific routes. Dynamic routing is generally more practical for larger networks with changing topology.

### 6. Explain the process of configuring a static route on a router.

**Answer:**

1. Identify the destination network.
2. Identify its subnet mask.
3. Identify a reachable next-hop router address or the correct exit interface.
4. Enter global configuration mode.
5. Configure the route.
6. Verify the route in the routing table.
7. Test connectivity to the destination.

**Example:** The destination is 8.8.8.8/32 and the next hop is 10.0.0.1.

~~~text
Router> enable
Router# configure terminal
Router(config)# ip route 8.8.8.8 255.255.255.255 10.0.0.1
Router(config)# end
Router# show ip route
Router# ping 8.8.8.8
~~~

A corresponding routing-table entry may look like:

~~~text
S 8.8.8.8/32 [1/0] via 10.0.0.1
~~~

- S means static.
- 8.8.8.8/32 is the destination host route.
- 1 is the administrative distance.
- 0 is the metric shown for this static route.
- 10.0.0.1 is the next hop.

The route is installed only if the next hop can be resolved through the routing table.

---

## Part 2 — Additional important interview questions

### 7. What is the difference between static routing and dynamic routing?

**Answer:**

| Static routing | Dynamic routing |
|---|---|
| Routes are configured manually. | Routes are learned through routing protocols. |
| Does not exchange routing updates. | Routers exchange routing information. |
| Suitable for small or stable networks. | Suitable for larger or changing networks. |
| Administrator maintains route changes. | Protocols can adapt to topology changes. |
| No routing-protocol processing overhead. | Uses CPU, memory, and bandwidth for protocol operation. |

### 8. What is a floating static route?

**Answer:**

A floating static route is a static route configured with a higher administrative distance than the preferred route to the same destination prefix. It acts as a backup. When the preferred route is removed from the routing table and the backup route is still reachable, the floating static route can be installed.

Example:

~~~text
Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.13.3 10
Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.14.4 20
~~~

- Primary next hop: 192.168.13.3, AD 10.
- Backup next hop: 192.168.14.4, AD 20.

The primary route is preferred because 10 is lower than 20.

### 9. What is the difference between a static route and a floating static route?

**Answer:**

A normal static route commonly uses the default AD of 1. A floating static route is deliberately configured with a higher AD so that it is less preferred than another route to the same prefix. It is used as a backup path.

### 10. What do S and S* mean in the Cisco routing table?

**Answer:**

- **S** identifies a static route.
- **S*** identifies a static candidate default route.

Example:

~~~text
S* 0.0.0.0/0 [10/0] via 192.168.13.3
~~~

This is a static candidate default route using next hop 192.168.13.3, with AD 10 and metric 0.

### 11. What is the gateway of last resort?

**Answer:**

The gateway of last resort is the next-hop router used when a packet's destination does not match a more-specific route and the router has a usable default route. Use show ip route to check it.

### 12. What is the default administrative distance of a static route?

**Answer:**

The default AD of a static route on Cisco routers is **1**. It can be changed by adding an administrative-distance value at the end of the static route command.

Example:

~~~text
ip route 0.0.0.0 0.0.0.0 192.168.13.3 10
~~~

Here, the static route has AD 10 instead of the default 1.

### 13. What is the difference between administrative distance and metric?

**Answer:**

Administrative distance compares the trust preference of different route sources for the same destination prefix. Metric is used by a routing protocol to compare paths according to that protocol's path-selection calculation.

For example:
- AD helps compare a static route with an OSPF route for the same prefix.
- OSPF cost helps OSPF choose between OSPF paths.

A lower AD is preferred when comparing route sources. A lower metric is generally preferred within the relevant routing protocol.

### 14. What is longest prefix match?

**Answer:**

Longest prefix match means the router selects the most specific route that matches the destination IP address.

For example, if a routing table contains 192.168.1.0/24 and 192.168.1.128/25, a packet destined for 192.168.1.150 matches both routes, but /25 is more specific and is selected.

### 15. What is the difference between a default route and a specific static route?

**Answer:**

A specific static route points to a particular destination network or host. A default route is used only when no more-specific matching route exists.

Specific route:

~~~text
ip route 192.168.20.0 255.255.255.0 10.0.0.1
~~~

Default route:

~~~text
ip route 0.0.0.0 0.0.0.0 10.0.0.1
~~~

### 16. What happens when the next-hop interface of a static route goes down?

**Answer:**

If the interface goes down and the route depends on that interface or its directly connected next hop, the connected route is removed and the static route may be removed from the routing table. A valid floating static route can then become the selected route.

However, a static route does not automatically detect every failure beyond its next hop. For remote-path failure detection, additional mechanisms such as IP SLA with object tracking may be needed.

### 17. How do you verify a static route?

**Answer:**

~~~text
show ip route
show ip route static
show ip interface brief
ping <destination-ip>
traceroute <destination-ip>
~~~

- show ip route displays the routing table.
- show ip route static displays static routes.
- show ip interface brief checks interface status and IP addresses.
- ping tests reachability.
- traceroute displays the Layer 3 path towards the destination.

### 18. Why is a default route commonly configured on an office router?

**Answer:**

An office router usually does not need to maintain an individual route for every destination on the Internet. It can forward unknown destinations to an upstream ISP router using a default route. The ISP has its own routing information to forward traffic further.

### 19. Can a router have more than one default route?

**Answer:**

Yes. A router can have multiple default routes. If they have different administrative distances, the route with the lower AD is preferred and the higher-AD route can act as a floating backup. If routes have equal preference, the router may install multiple paths depending on the configuration and platform.

### 20. What is the purpose of the administrative-distance value at the end of the static route command?

**Answer:**

It changes the route's administrative distance.

~~~text
ip route 0.0.0.0 0.0.0.0 192.168.14.4 20
~~~

The value 20 sets the AD to 20. This can make the route less preferred than another route with a lower AD.

---

## Part 3 — Lab-based interview questions

### 21. What was the objective of the this lab?

**Answer:**

The objective was to provide connectivity from the office PC to the simulated destination 8.8.8.8 using static and default routing, then verify the primary and backup ISP paths.

### 22. Which ISP was configured as the primary path and which was the backup?

**Answer:**

- Airtel was the primary path.
- Tata was the backup path.

The default route towards Airtel was configured with a lower AD than the Tata route.

### 23. Which next hop was used for the Airtel path in the lab?

**Answer:**

The R1 routing-table output showed the Airtel next hop as 192.168.13.3.

The captured entry was:

~~~text
S* 0.0.0.0/0 [10/0] via 192.168.13.3
~~~

### 24. Which next hop was used for the Tata backup path?

**Answer:**

The Tata next hop shown in the lab traceroute was 192.168.14.4.

### 25. How did you test failover from Airtel to Tata?

**Answer:**

1. PC1 initially reached 8.8.8.8 through Airtel.
2. The initial traceroute showed 192.168.13.3 as the Airtel hop.
3. The Airtel-facing Ethernet3/3 interface on R1 was shut down.
4. A new traceroute was run from PC1.
5. The path then showed 192.168.14.4, which is the Tata path.

This demonstrated a change in the forwarding path from Airtel to Tata.

### 26. Which commands were used during the failover test?

**Answer:**

~~~text
show ip route
configure terminal
interface e3/3
shutdown
no shutdown
~~~

Traceroute was run from PC1 to verify the path.

### 27. What does [10/0] mean in the lab's routing-table output?

**Answer:**

In the route entry:

~~~text
S* 0.0.0.0/0 [10/0] via 192.168.13.3
~~~

- 10 is the administrative distance.
- 0 is the metric.

### 28. Does pinging 8.8.8.8 in this lab prove that the PC reached the real public Google DNS server?

**Answer:**

No. In this lab topology, 8.8.8.8 is configured as a loopback destination on R5. The successful ping proves connectivity to that simulated destination inside the lab. It does not prove access to the real public Internet or demonstrate NAT configuration.

### 29. Why did the lab use static/default routing instead of BGP between the ISP routers?

**Answer:**

The lab uses static and default routes to demonstrate primary and backup forwarding paths. In real ISP-to-ISP Internet routing, BGP is used to exchange routes between autonomous systems.

### 30. What evidence showed that the backup path was working?

**Answer:**

The later PC traceroute showed the Tata next hop 192.168.14.4 and the destination-side hop 192.168.45.5. The continuous ping screenshot also showed successful replies during the test after the path change.

The screenshots demonstrate the observed forwarding-path change and successful reachability. They do not show every internal routing-table change at the exact moment of failover.

---

## Part 4 — Practical revision questions

Try answering these without looking at the answers first.

### Q1. Configure a static route

A router must reach 192.168.20.0/24 through next hop 10.0.0.2. Write the command.

<details>
<summary>Show answer</summary>

~~~text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
~~~
</details>

### Q2. Configure a default route

A router must forward unknown IPv4 destinations to 10.1.1.1. Write the command.

<details>
<summary>Show answer</summary>

~~~text
ip route 0.0.0.0 0.0.0.0 10.1.1.1
~~~
</details>

### Q3. Which route is preferred?

Two routes exist for the same prefix. Route A has AD 5 and Route B has AD 25.

<details>
<summary>Show answer</summary>

Route A is preferred because it has the lower administrative distance, 5.
</details>

### Q4. Which route is preferred: a /32 OSPF route or a /24 static route?

Both routes match the destination IP address. The OSPF route has AD 110 and the static route has AD 1.

<details>
<summary>Show answer</summary>

The /32 OSPF route is selected because it is more specific than the /24 static route. Longest prefix match is evaluated before administrative distance.
</details>

### Q5. Which route is preferred when both routes have the same prefix?

- Static route: 192.168.10.0/24, AD 1.
- OSPF route: 192.168.10.0/24, AD 110.

<details>
<summary>Show answer</summary>

The static route is preferred because both prefixes are /24, and the static route has the lower AD of 1.
</details>

### Q6. Interpret this route entry.

~~~text
S* 0.0.0.0/0 [10/0] via 192.168.13.3
~~~

<details>
<summary>Show answer</summary>

It is a static candidate default route. Its AD is 10, metric is 0, and next hop is 192.168.13.3.
</details>

### Q7. What happens if the primary default route is removed and a valid floating static default route exists?

<details>
<summary>Show answer</summary>

The floating static route can be installed in the routing table and used as the default path, provided its next hop remains reachable.
</details>

### Q8. In the this lab, which path was active when traceroute showed 192.168.13.3?

<details>
<summary>Show answer</summary>

Airtel was the active path. 192.168.13.3 was the Airtel next hop.
</details>

### Q9. In the this lab, which path was active when traceroute showed 192.168.14.4?

<details>
<summary>Show answer</summary>

Tata was the active backup path. 192.168.14.4 was the Tata next hop.
</details>

### Q10. Which command displays the routing table?

<details>
<summary>Show answer</summary>

~~~text
show ip route
~~~
</details>

### Q11. What does the router do if it has no matching route and no default route?

<details>
<summary>Show answer</summary>

The router drops the packet because it has no route that tells it where to forward the packet.
</details>

### Q12. Does a floating static route guarantee failover for every type of ISP failure?

<details>
<summary>Show answer</summary>

No. A floating static route can take over when the preferred route is removed from the routing table. A failure beyond the directly connected next hop may not remove the primary static route automatically. Additional failure detection, such as IP SLA with object tracking, may be needed.
</details>

---

## Quick command revision

| Purpose | Command |
|---|---|
| Configure a specific static route | ip route <network> <mask> <next-hop> |
| Configure a default route | ip route 0.0.0.0 0.0.0.0 <next-hop> |
| Configure a floating default route | ip route 0.0.0.0 0.0.0.0 <next-hop> <AD> |
| View routing table | show ip route |
| View static routes | show ip route static |
| Check interface status | show ip interface brief |
| Test reachability | ping <destination-ip> |
| Check packet path | traceroute <destination-ip> |
| Enter global configuration | configure terminal |
| Enter interface configuration | interface <interface-id> |
| Disable an interface | shutdown |
| Enable an interface | no shutdown |

## Key points to remember

- Longest prefix match is checked before administrative distance.
- Lower AD is preferred when comparing routes to the same prefix.
- Static route default AD is 1.
- IPv4 default route is 0.0.0.0/0.
- S means static route; S* marks a candidate default route.
- A floating static route has a higher AD than the preferred route.
- In the this lab, Airtel was primary and Tata was backup.
- The lab's 8.8.8.8 was a simulated loopback destination on R5, not proof of public Internet access.
