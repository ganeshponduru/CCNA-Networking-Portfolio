# Different-Subnet Ping — Verification Checklist

I use this checklist when checking a routed VPCS lab.

- [ ] Confirm PC3 address and subnet mask.
- [ ] Confirm PC3 default gateway is `192.168.1.100`.
- [ ] Confirm R1 G1/0 is `192.168.1.100/24` and operational.
- [ ] Confirm R1 G2/0 is `192.168.20.200/24` and operational.
- [ ] Confirm PC4 address and subnet mask.
- [ ] Confirm PC4 default gateway is `192.168.20.200`.
- [ ] Ping PC4 from PC3 and record the actual output.
- [ ] Capture traffic on the PC3-to-R1 link to check ARP for `192.168.1.100` and the ICMP packet.
- [ ] Capture traffic on the R1-to-PC4 link to check ARP for `192.168.20.1` if needed and the forwarded ICMP packet.
- [ ] Compare Ethernet source/destination MAC addresses on each link and the IP source/destination addresses.
- [ ] Record any failures and the troubleshooting steps used.

I should only mark a checklist item complete after checking it in the lab and keeping the evidence.
