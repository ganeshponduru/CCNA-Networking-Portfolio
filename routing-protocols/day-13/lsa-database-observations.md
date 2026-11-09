# LSA Database Observations

The following observations are taken from the OSPF database outputs captured in the lab. The database is compared across Area 0, Area 10, and Area 20.

## R1 – Area 0

The visible database contains:

- **Router Link States (Type 1):** Router IDs 1.1.1.1, 2.2.2.2, 3.3.3.3, and 4.4.4.4.
- **Net Link States (Type 2):** Link IDs 192.168.12.2, 192.168.13.3, and 192.168.24.4.
- **Summary Net Link States (Type 3):** Inter-area entries including 5.5.5.5, 6.6.6.6, 7.7.7.7, and the visible remote network prefixes.

## R5 – Area 10

The visible database contains:

- **Router Link States (Type 1):** Router IDs 4.4.4.4 and 5.5.5.5.
- **Net Link States (Type 2):** Link ID 192.168.45.5.
- **Summary Net Link States (Type 3):** Entries advertising prefixes from other areas.

The Type 3 advertisements into Area 10 are associated with the ABR path through R4 (Router ID 4.4.4.4).

## R7 – Area 20

The visible database contains:

- **Router Link States (Type 1):** Router IDs 3.3.3.3, 6.6.6.6, and 7.7.7.7.
- **Net Link States (Type 2):** Link IDs 192.168.36.6 and 192.168.67.7.
- **Summary Net Link States (Type 3):** Entries advertising prefixes from other areas.

The Type 3 advertisements into Area 20 are associated with the ABR path through R3 (Router ID 3.3.3.3).

## R2 – Area 0

The captured R2 database output shows the Area 0 Router Link States, Net Link States, and Summary Net Link States. R2's visible OSPF interfaces were both assigned to Area 0, so this output does not establish R2 as an ABR.

## R3 – Area 0 and Area 20

The captured R3 output shows database information for Area 0 and Area 20. This is consistent with R3 participating in both areas and acting as the ABR between them.

## What the comparison shows

1. Type 1 LSAs describe routers within their respective areas.
2. Type 2 LSAs describe multi-access networks where a DR is elected.
3. Type 3 LSAs provide inter-area prefix information.
4. Routers in different areas do not have identical area-specific LSDB contents.
5. The ABR roles in this actual configuration are R3 and R4.

The screenshot set does not include a separate, complete R2 Area 10 database view. No claim is made that such an R2 Area 10 database was verified.
