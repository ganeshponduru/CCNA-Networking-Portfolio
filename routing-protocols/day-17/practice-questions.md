# Practice and interview questions

## EIGRP

**1. What is EIGRP?**

EIGRP is an advanced distance-vector routing protocol that uses DUAL to select loop-free routes.

**2. What is the purpose of an EIGRP Hello packet?**

Hello packets discover and maintain neighbor relationships.

**3. What does an EIGRP Update packet carry?**

It carries routing information that is sent to EIGRP neighbors.

**4. What is the purpose of an EIGRP acknowledgment?**

It confirms receipt of reliably delivered EIGRP information, such as an Update.

**5. Which IP protocol number and multicast address are used by EIGRP for IPv4?**

IP protocol 88 and multicast address 224.0.0.10.

**6. What is the EIGRP AS number used in this lab?**

AS 90.

## VRRP

**7. Why is VRRP used?**

It provides redundancy for the default gateway so another router can take over if the Master becomes unavailable.

**8. What are the two main VRRP router roles?**

Master and Backup.

**9. Which virtual IP was used in this lab?**

192.168.1.150.

**10. What happened when R1's LAN interface was shut down?**

R1 moved from Master to Init, and R2 changed from Backup to Master.

**11. Which IPv4 multicast address and IP protocol number were visible in the VRRP capture?**

224.0.0.18 and IP protocol 112.

## HSRP

**12. What are the HSRP router roles?**

Active and Standby.

**13. Which virtual IP was used in the HSRP test?**

192.168.1.150.

**14. What happened when R1's LAN interface was shut down?**

R2 changed from Standby to Active and became the first hop for PC1.

**15. Which UDP port was visible in the HSRP capture?**

UDP port 1985.

## Compare

**16. What is the main terminology difference between VRRP and HSRP?**

VRRP uses Master/Backup, while HSRP uses Active/Standby.

**17. Did the failover tests have zero packet loss?**

No. A brief ping timeout was visible during the transition, followed by successful replies.
