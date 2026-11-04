# Day 10 — Important Interview and Practice Questions

These questions cover the static-routing and default-routing topics from the class and the dual-ISP lab.

## Important interview questions

1. What is a static route?
2. What is a default route?
3. What is the IPv4 default route?
4. What does the gateway of last resort mean?
5. What is administrative distance?
6. Why is a lower administrative distance preferred?
7. What is a floating static route?
8. What is the difference between a static route and a default route?
9. What does the `S` code represent in `show ip route`?
10. What does the `S*` code represent?
11. What is the purpose of the `show ip route` command?
12. How can traceroute help identify the path taken by packets?
13. In a dual-ISP setup, how can one ISP be configured as the primary path and another as the backup?
14. What happens to a directly connected route when its interface is shut down?
15. Does a successful ping to a lab loopback address prove that the PC has access to the public Internet?

## Practice questions

### Q1. Write the default route command

A router should forward unknown IPv4 destinations to next hop `10.1.1.1`. Write the command.

<details>
<summary>Answer</summary>

```text
ip route 0.0.0.0 0.0.0.0 10.1.1.1
```
</details>

### Q2. Choose the preferred route

Two static routes exist for the same destination prefix:
- Route A has AD 5.
- Route B has AD 25.

Which route is preferred?

<details>
<summary>Answer</summary>

Route A, because the lower administrative distance is preferred.
</details>

### Q3. Identify the default route

What does this routing-table entry mean?

```text
S* 0.0.0.0/0 [10/0] via 192.168.13.3
```

<details>
<summary>Answer</summary>

It is a static candidate default route with administrative distance 10 and metric 0, using `192.168.13.3` as the next hop.
</details>

### Q4. Interpret the lab traceroute

The first traceroute shows `192.168.13.3` as the second hop. Which ISP path is being used?

<details>
<summary>Answer</summary>

Airtel, represented by R3 at `192.168.13.3`.
</details>

### Q5. Interpret the failover traceroute

After R1's Ethernet3/3 interface is shut down, the second hop becomes `192.168.14.4`. Which path is being used?

<details>
<summary>Answer</summary>

Tata, represented by R4 at `192.168.14.4`.
</details>

### Q6. Does `8.8.8.8` prove public Internet access in this lab?

<details>
<summary>Answer</summary>

No. In this topology, `8.8.8.8` is configured as a loopback address on R5. The test verifies reachability to that simulated destination inside the lab.
</details>

## Commands for revision

```text
show ip route
ping 8.8.8.8
trace 8.8.8.8
config terminal
interface e3/3
shutdown
no shutdown
```
