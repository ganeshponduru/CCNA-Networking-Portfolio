# Routing Protocols

This section contains my notes and lab work on routing. Each topic includes the concepts, configurations visible in the lab, routing-table observations, and verification results.

## Day 10 — Static and default routing

The Day 10 lab uses a dual-ISP topology with R3 (Airtel) and R4 (Tata) connected to R1, and both paths leading toward R5 (Google).

- [Static routing](day-10/static-routing.md) — topology, static routes on R3/R4/R5, routing-table observations, and connectivity tests.
- [Default routing](day-10/default-routing.md) — R1's installed default route via R3 and PC1's connectivity test to `8.8.8.8`.

**Lab note:** The current R1 screenshot shows one default route via Airtel. A backup default route via Tata and an R1 failover test are not included in the captured evidence yet.
