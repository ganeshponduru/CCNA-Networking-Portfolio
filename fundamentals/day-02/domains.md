# Collision Domains and Broadcast Domains

I keep collision domains and broadcast domains separate in my notes because they describe different things and are affected by devices differently.
## Collision domain
A collision domain is a shared Ethernet segment where simultaneous transmissions can collide in half-duplex operation.

- **Hub:** Layer 1 repeater; repeats incoming signals out other ports and does not learn MAC addresses. All ports on a hub share one collision domain.
- **Switch:** separates collision domains per port. On modern full-duplex switched Ethernet, collisions do not occur in normal operation because each link can transmit and receive simultaneously.

### CSMA/CD
Carrier Sense Multiple Access with Collision Detection was used by shared, half-duplex Ethernet. A device senses the medium, transmits if available, detects a collision, sends a jam signal, waits a randomized backoff interval, and retries. It does not simply hold packets until another computer finishes a task. CSMA/CD is not used for normal full-duplex switched Ethernet.

## Broadcast domain
A broadcast domain is the Layer 2 area within which a broadcast frame is flooded.
- A Layer 2 switch forwards broadcasts within the same VLAN.
- Each VLAN is a separate broadcast domain.
- A router interface normally separates broadcast domains; routers do not forward ordinary Layer 2 broadcasts to another interface.
- A Layer 3 switch can route between VLANs when configured.

| Device/configuration | Collision domains | Broadcast domains |
|---|---|---|
| Hub | All ports share one | One shared domain |
| Switch, one VLAN, full duplex | One per port; no collisions in normal full-duplex operation | One per VLAN |
| Router | Each interface is a separate link/domain in a typical Ethernet LAN | Each routed interface/subnet is separate |

## Repeater note
A repeater is a physical-layer device that regenerates or retimes signals; it is not simply a passive cable joiner. Cable limits depend on the Ethernet standard and installation. Common twisted-pair Ethernet channels are typically limited to 100 m.