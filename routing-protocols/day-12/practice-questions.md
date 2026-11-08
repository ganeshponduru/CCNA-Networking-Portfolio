# OSPF Adjacency Process — Practice and Interview Questions

## Fundamentals

**1. What is an OSPF neighbor?**  
An OSPF neighbor is another OSPF-speaking router with which a router exchanges Hello packets and establishes a neighbor relationship when the required parameters match.

**2. What are the five OSPF packet types?**  
Hello, Database Description (DBD), Link State Request (LSR), Link State Update (LSU), and Link State Acknowledgment (LSAck).

**3. What is the purpose of a Hello packet?**  
It discovers and maintains neighbors. It carries parameters such as the network mask, Hello and Dead intervals, options, router priority, DR/BDR information, and the neighbor list.

**4. What is the OSPF common header size in OSPFv2?**  
The OSPFv2 common header is 24 bytes.

**5. What is the purpose of a DBD packet?**  
It summarizes the sender's link-state database by carrying LSA headers. It helps routers identify which LSAs they need.

**6. Does a DBD packet carry complete LSAs?**  
No. It carries LSA headers. Complete LSA information is delivered in LSU packets.

**7. What is an LSR packet used for?**  
It requests specific missing or newer LSAs from a neighbor.

**8. What is an LSU packet used for?**  
It carries one or more complete LSAs to a neighbor.

**9. What is an LSAck packet used for?**  
It acknowledges received LSAs and supports reliable LSA flooding.

## Neighbor states and adjacency

**10. What are the OSPF neighbor states?**  
Down, Attempt, Init, 2-Way, ExStart, Exchange, Loading, and Full. Attempt is used for NBMA neighbor discovery; the common progression on broadcast and point-to-point networks does not include it.

**11. What happens in the Init state?**  
A router has received a Hello from the neighbor, but its own Router ID is not yet listed in that neighbor's Hello packet.

**12. What does the 2-Way state confirm?**  
It confirms bidirectional Hello communication: each router sees the other's Router ID in its Hello packet.

**13. What happens during ExStart?**  
Routers negotiate master/slave roles and initial DBD sequence numbers.

**14. What happens during Exchange?**  
Routers exchange DBD packets containing LSA headers that summarize their link-state databases.

**15. What happens during Loading?**  
Routers request missing or newer LSAs using LSR packets and receive them in LSU packets.

**16. What does Full mean?**  
The adjacent routers have synchronized their link-state databases.

**17. Do all OSPF neighbors on an Ethernet broadcast network reach Full with one another?**  
No. DROTHER routers can remain 2-Way with each other. They form Full adjacencies with the DR and BDR.

**18. What is the difference between a neighbor relationship and an adjacency?**  
A neighbor relationship is established through Hello communication. An adjacency is a neighbor relationship in which routers synchronize their link-state databases.

## Master/slave and DR/BDR

**19. How are OSPF DBD master and slave roles selected?**  
During ExStart, the router with the higher Router ID normally becomes the master. The master controls the DBD sequence numbers.

**20. Is the DBD master always the DR?**  
No. DBD master/slave negotiation is separate from DR/BDR election.

**21. How is the DR elected?**  
The highest interface priority wins. If priorities tie, the highest Router ID wins. An interface with priority 0 is not eligible.

**22. Why does OSPF elect a DR and BDR on broadcast networks?**  
They reduce the number of required adjacencies and help organize link-state advertisement exchange on a multiaccess segment.

## Troubleshooting and verification

**23. What can cause OSPF neighbors to remain in Init?**  
One-way Hello communication is a common cause. Check whether the local Router ID appears in the neighbor's Hello packet, along with network connectivity and interface settings.

**24. What can cause an adjacency to get stuck in ExStart or Exchange?**  
An MTU mismatch is a common cause. Also verify interface connectivity, OSPF settings, packet exchange, and platform-specific behavior.

**25. What should be checked if OSPF neighbors do not reach Full?**  
Check the neighbor state, Hello and Dead intervals, area ID, network type, authentication, subnet and mask, interface status, MTU, and relevant OSPF logs or packet captures.

**26. Which commands help verify OSPF operation?**  
Use `show ip ospf neighbor`, `show ip ospf interface`, `show ip ospf database`, `show ip route`, and `show ip protocols`.

**27. What did the packet capture show in the lab?**  
It showed Hello, DBD, LSR, LSU, and LSAck packets exchanged between R1 and R2. The LSR requested R2's Router-LSA, the LSU carried R1's Router-LSA, and an LSAck acknowledged an LSA header advertised by R2.

**28. What routes were visible in the lab routing tables?**  
R1 learned R2's loopback 2.2.2.2/32, and R2 learned R1's loopback 1.1.1.1/32. Both routers had the connected 192.168.1.0/24 network.
