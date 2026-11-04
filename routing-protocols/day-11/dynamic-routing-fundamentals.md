# Dynamic Routing Fundamentals

## Why dynamic routing is needed

With static routing, an administrator manually configures the destination network, subnet mask, and next hop. Every router that needs a route must be configured appropriately. This can work well in small or stable networks, but the configuration effort grows as the network expands.

Dynamic routing protocols allow routers to exchange routing information and learn remote networks automatically. The administrator enables the protocol and identifies the networks or interfaces that should participate. The routers then discover neighbors, exchange routing information, calculate paths, and update their routing tables as the topology changes.

## Static routing vs dynamic routing

| Static routing | Dynamic routing |
|---|---|
| Routes are entered manually. | Routes are learned through a routing protocol. |
| Low protocol overhead. | Uses CPU, memory, and bandwidth for protocol operation. |
| Suitable for small, stub, or predictable networks. | Scales better in networks with multiple routers and changing paths. |
| Changes generally require manual intervention unless tracking or another mechanism is configured. | Can detect topology changes and recalculate routes. |
| Requires administrators to maintain route information. | Requires protocol knowledge and correct neighbor/network configuration. |

## General route-learning process

The exact process differs between routing protocols, but the general sequence is:

1. **Neighbor discovery:** Routers use protocol-specific messages, such as OSPF Hello packets, to discover eligible neighboring routers.
2. **Neighbor relationship:** Routers establish the required neighbor or adjacency relationship. Matching protocol parameters and working links are important.
3. **Routing information exchange:** Routers exchange information about reachable networks or topology.
4. **Path calculation:** The routing protocol applies its algorithm and metric to select the best path or paths.
5. **Routing-table installation:** The selected routes are installed in the IP routing table when they are preferred over competing routes.
6. **Topology-change handling:** When a link or router changes state, the protocol communicates the change and recalculates affected routes.

A routing protocol does not simply copy another router's entire routing table in every case. OSPF, for example, distributes link-state information and calculates routes from its link-state database.

## Autonomous Systems and ASNs

An **Autonomous System (AS)** is a group of IP networks and routers managed under a common routing policy. An **Autonomous System Number (ASN)** identifies that AS for inter-domain routing.

Two common ASN formats are:

- 2-byte ASN: 16 bits.
- 4-byte ASN: 32 bits.

The commonly used 2-byte private ASN range is 64512–65534. The range 0 and 65535 is reserved, and 64496–64511 is reserved for documentation. For 4-byte ASNs, the private range is 4200000000–4294967294.

An OSPF process ID is not an ASN. It identifies an OSPF process locally on a router.

## IGP and EGP

Routing protocols are commonly grouped by where they operate in relation to autonomous systems.

| Category | Purpose | Examples |
|---|---|---|
| Interior Gateway Protocol (IGP) | Exchanges routes within one autonomous system. | OSPF, IS-IS, EIGRP, RIP |
| Exterior Gateway Protocol (EGP) | Exchanges routing information between autonomous systems. | BGP |

BGP (Border Gateway Protocol) is the inter-domain routing protocol used to exchange reachability information between autonomous systems. The term EGP can also refer to a historical protocol, so it is clearer to say **IGP vs inter-domain BGP** when discussing modern networks.

## Common IGP types

| Protocol | General classification | Key point |
|---|---|---|
| RIP | Distance vector | Uses hop count; maximum usable distance is 15 hops. |
| EIGRP | Advanced distance vector / hybrid terminology is sometimes used | Uses a composite metric and the DUAL algorithm. |
| OSPF | Link state | Uses a link-state database and Dijkstra's SPF algorithm. |
| IS-IS | Link state | Commonly used in large service-provider networks. |

RIP's hop-count limit makes it unsuitable for many large networks. It remains useful to understand as a foundational routing protocol.

## OSPF overview

**OSPF (Open Shortest Path First)** is a link-state Interior Gateway Protocol.

OSPF routers build a view of the topology by exchanging Link-State Advertisements (LSAs). Routers in the same area maintain a synchronized link-state database for that area. Each router independently runs the Shortest Path First (SPF) algorithm, based on Dijkstra's algorithm, to calculate the shortest paths.

OSPF uses a metric called **cost**. The cumulative cost along a path is used to compare paths to a destination. OSPF can install multiple paths when they have equal cost and meet the protocol's route-installation conditions.

OSPF is not limited to sending updates only when a change occurs. It also sends periodic Hello packets, and LSAs have refresh and aging behavior. Topology changes trigger new link-state information and SPF calculation as required.

## Advantages and disadvantages of dynamic routing

**Advantages**
- Reduces the need to maintain large numbers of static routes manually.
- Can adapt to topology changes.
- Supports larger and more complex network designs.
- Can provide alternate paths and equal-cost multipath forwarding.

**Disadvantages**
- Requires correct protocol design and configuration.
- Consumes router CPU and memory.
- Protocol messages use network bandwidth.
- Misconfiguration can cause adjacency failures, route instability, or unintended route propagation.

## Important distinctions

- A routing protocol learns and advertises routes; the routing table contains the routes selected for forwarding.
- Neighbor discovery does not automatically mean all routes are installed. The routers must exchange the required information and calculate valid paths.
- The OSPF process ID is locally significant. Neighboring routers do not have to use the same process ID.
- OSPF area IDs must match on the interfaces forming an adjacency.
- The OSPF backbone is Area 0. Multi-area OSPF designs use the backbone to connect non-backbone areas.
