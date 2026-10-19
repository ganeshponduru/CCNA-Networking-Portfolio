# Day 7 — Practice Questions

I use these questions to check whether I can explain the hardware concepts without just memorising terms.

## Questions

1. What is the difference between a power cable and a PSU?
2. What does 1+1 PSU redundancy mean, and why should I check the device-specific design?
3. What does SFP stand for, and what is the role of a transceiver?
4. Why is it incorrect to say that every SFP supports a 40 km link?
5. What is the difference between single-mode and multimode fiber?
6. Name several fiber connector families.
7. What do Tx and Rx mean on a duplex optical link?
8. What is a fiber distribution panel used for?
9. What is fiber splicing, and why is it not necessarily performed at a fixed interval?
10. What is the difference between a fixed-port device and a modular chassis?
11. What does a line card generally provide?
12. Why do hardware module names differ between vendors?
13. What is the purpose of a fan tray?
14. What is the difference between console access and SSH access?
15. If an interface stays down, what would I check before replacing the optic?
16. What evidence should I collect before escalating a suspected fiber fault?
17. What does FRU mean? What is an RMA process?
18. Why should I check the hardware guide before removing a module?

## Short answers

1. The cable carries power from the source; the PSU converts it into power the device electronics require.
2. It generally means two supplies are arranged for redundancy, but the exact power-sharing/failover behavior depends on the platform.
3. Small Form-factor Pluggable; it provides the supported optical or copper interface for a link.
4. Reach depends on the exact transceiver, fiber, wavelength, link loss, and platform specifications.
5. Single-mode is commonly used for longer distances; multimode is commonly used for shorter links.
6. LC, SC, ST, FC, and MPO/MTP.
7. Transmit and receive.
8. It organizes, terminates, splices, and patches fibers between outside-plant cabling and equipment.
9. Splicing joins fibers; its location depends on cable installation and repair needs.
10. Fixed-port interfaces are built in; a modular chassis accepts replaceable interface/control modules.
11. It provides network interfaces and associated platform-specific resources.
12. Hardware architecture and naming conventions vary by product family.
13. It maintains airflow and cooling.
14. Console is local physical management; SSH is encrypted remote management over IP.
15. Both-end status, admin state, compatibility, patch lead, connector cleanliness, polarity, fiber type, alarms, and optical levels where available.
16. Interface state, logs, transceiver details/diagnostics, both endpoint IDs, patch-panel path, and tests already performed.
17. Field-Replaceable Unit; Return Merchandise Authorization.
18. To avoid damage, service disruption, or unsafe removal and to follow the correct procedure.
