# OSPF External Route Redistribution – Practice Questions

## Questions and answers

**1. What is route redistribution?**  
Route redistribution injects routes learned by one routing protocol into another routing protocol.

**2. Why is redistribution needed in this lab?**  
R1 connects the OSPF domain to an external BGP autonomous system. Redistribution allows the external BGP prefix to be advertised through OSPF and internal OSPF routes to be advertised through BGP.

**3. What is an ASBR?**  
An Autonomous System Boundary Router is an OSPF router that injects external routing information into the OSPF domain through redistribution or another external route origination mechanism.

**4. Which router acts as the ASBR in this lab?**  
R1 (Router ID 1.1.1.1).

**5. Why does R1 become an ASBR?**  
R1 redistributes the BGP route for 8.8.8.8/32 into OSPF.

**6. What is an ABR?**  
An Area Border Router participates in multiple OSPF areas and connects their area databases through inter-area routing.

**7. Which routers are ABRs in this topology?**  
R2 connects Area 0 and Area 10. R3 connects Area 0 and Area 20.

**8. What does the B bit in a Type 1 Router-LSA indicate?**  
It indicates that the advertising router is an Area Border Router.

**9. What does the E bit in a Type 1 Router-LSA indicate?**  
It indicates that the advertising router is an AS Boundary Router.

**10. What is a Type 5 LSA?**  
A Type 5 AS External LSA advertises an external prefix into the OSPF domain.

**11. Who originates a Type 5 LSA?**  
The ASBR originates the Type 5 LSA.

**12. What does the Link State ID represent in the Type 5 LSA observed here?**  
It identifies the external prefix; the captured Link State ID is 8.8.8.8.

**13. What is a Type 4 LSA?**  
A Type 4 ASBR Summary LSA advertises reachability information for an ASBR into another OSPF area.

**14. Who originates a Type 4 LSA?**  
An ABR originates a Type 4 LSA into the relevant area.

**15. What does the Link State ID represent in a Type 4 LSA?**  
It identifies the Router ID of the ASBR.

**16. In Area 10, which router advertises the Type 4 LSA for R1?**  
R2 (Router ID 2.2.2.2).

**17. In Area 20, which router advertises the Type 4 LSA for R1?**  
R3 (Router ID 3.3.3.3).

**18. What is the difference between Type 4 and Type 5 LSAs?**  
Type 4 provides information to reach the ASBR. Type 5 advertises the external destination prefix.

**19. What is an E2 route?**  
An OSPF External Type 2 route uses the external metric as its primary metric. Internal cost to reach the ASBR is not added to that external metric.

**20. What is an E1 route?**  
An OSPF External Type 1 route uses the external metric plus the internal OSPF cost to reach the ASBR.

**21. Which external route type was installed on R5 in this lab?**  
E2, shown by the route code `O E2`.

**22. What does `[110/1]` mean in the R5 route entry?**  
110 is the OSPF administrative distance and 1 is the route metric shown for the E2 external route.

**23. What does the next hop 192.168.45.4 represent?**  
It is the next-hop address of R4 toward the ASBR.

**24. What is the purpose of the command `redistribute bgp 100` under OSPF?**  
It injects eligible routes from BGP AS 100 into OSPF.

**25. What is the purpose of `redistribute ospf 110` under BGP?**  
It injects eligible routes from OSPF process 110 into BGP.

**26. How can the OSPF external LSAs be checked?**  
Use `show ip ospf database external`.

**27. How can the Type 4 LSAs be checked?**  
Use `show ip ospf database asbr-summary`.

**28. How can the installed route to 8.8.8.8 be checked?**  
Use `show ip route 8.8.8.8`.

**29. Does the lab test access to the real public Google DNS service?**  
No. The simulated R8 router has Loopback0 configured as 8.8.8.8/32. The ping verifies reachability to that lab loopback.

**30. Why should redistribution be controlled in a production network?**  
Unfiltered redistribution can introduce unwanted routes, create routing feedback or loops, and produce suboptimal paths. Route maps, prefix lists, tags, and explicit metrics can be used to control what is redistributed.
