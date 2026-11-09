# OSPF Multi-Area and LSA Practice Questions

## Questions and answers

**1. What is an LSA in OSPF?**  
An LSA is a Link-State Advertisement. It carries topology or reachability information that OSPF routers use to build their link-state databases.

**2. What is an LSDB?**  
The Link-State Database is the collection of LSAs maintained by OSPF routers for an area. Routers in the same area should have synchronized area-specific LSDB information.

**3. What information does a Type 1 LSA carry?**  
It describes a router's OSPF links and link characteristics within an area.

**4. Which routers generate Type 1 LSAs?**  
Every OSPF router generates a Type 1 LSA for each area in which it participates.

**5. What is a Type 2 LSA?**  
A Network LSA describes a multi-access network and the routers attached to it.

**6. Who generates a Type 2 LSA?**  
The DR generates it on a broadcast or NBMA multi-access network.

**7. Is a Type 2 LSA generated on every OSPF link?**  
No. Point-to-point links do not elect a DR and do not generate Type 2 LSAs.

**8. What is a Type 3 LSA?**  
A Summary LSA advertises network prefixes from one OSPF area into another.

**9. Who originates Type 3 LSAs?**  
An ABR originates Type 3 LSAs to advertise inter-area prefixes.

**10. Does an ABR convert Type 1 and Type 2 LSAs directly into Type 3 LSAs?**  
No. It uses the inter-area reachability information to originate Type 3 Summary LSAs. This is not a literal conversion of the original LSAs.

**11. What is an OSPF ABR?**  
An Area Border Router participates in multiple OSPF areas and connects those areas. In the normal hierarchical design, it connects a non-backbone area to Area 0.

**12. Does a router become an ABR because it is physically between two other routers?**  
No. Its OSPF interface area assignments determine the areas in which it participates.

**13. Why is Area 0 important?**  
Area 0 is the OSPF backbone. Inter-area routing is designed to pass through the backbone.

**14. Why are OSPF areas used?**  
Areas limit the scope of detailed link-state information and help make larger OSPF networks more scalable.

**15. Where can Type 1 LSAs be seen in the CLI output?**  
Under Router Link States in `show ip ospf database`.

**16. Where can Type 2 LSAs be seen in the CLI output?**  
Under Net Link States in `show ip ospf database`.

**17. Where can Type 3 LSAs be seen in the CLI output?**  
Under Summary Net Link States in `show ip ospf database`.

**18. In this lab configuration, which routers act as ABRs?**  
R3 connects Area 0 and Area 20. R4 connects Area 0 and Area 10.

**19. Why is R2 not an ABR in this configuration?**  
Both visible OSPF interfaces on R2 were assigned to Area 0. It therefore participates only in Area 0 in this configuration.

**20. What changed when the R2–R4 link was assigned to Area 0?**  
R2 remained an internal Area 0 router, and R4 became the ABR between Area 0 and Area 10. The OSPF area assignment changed the area participation and ABR role.

**21. Which command displays the OSPF link-state database?**  
`show ip ospf database`.

**22. Which command displays the OSPF area assignment for interfaces?**  
`show ip ospf interface brief`.
