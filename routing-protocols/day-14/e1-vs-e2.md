# OSPF External Route Types: E1 and E2

When an ASBR redistributes an external route into OSPF, the external route can be advertised as External Type 1 (E1) or External Type 2 (E2).

## E2 – External Type 2

E2 is the default external route type on many Cisco OSPF redistribution configurations.

- The external metric advertised by the ASBR is the primary metric used for route comparison.
- The internal OSPF cost to reach the ASBR is not added to the displayed external metric.
- Internal cost is still relevant when OSPF determines the path to the ASBR and resolves competing paths.

## E1 – External Type 1

- The total route metric includes the external metric plus the internal OSPF cost to reach the ASBR.
- As the internal path to the ASBR changes, the total E1 metric can change accordingly.

## Practical output from this lab

R5's routing table contains:

```text
O E2 8.8.8.8 [110/1] via 192.168.45.4, GigabitEthernet1/0
```

| Field | Meaning |
|---|---|
| O | OSPF route |
| E2 | External Type 2 |
| 8.8.8.8 | External destination |
| 110 | OSPF administrative distance |
| 1 | External route metric shown |
| 192.168.45.4 | Next hop toward the ASBR |
| GigabitEthernet1/0 | Outgoing interface |

The captured output confirms E2 behavior only. An E1 route was not configured or captured in this lab, so no practical E1 result is claimed.

## Configuration reference

To set the external metric type during redistribution, Cisco IOS supports the `metric-type` option on the redistribution command. For example:

```text
router ospf 110
 redistribute bgp 100 subnets metric-type 1
```

This is a reference example only; the lab output provided here shows an E2 route, not this E1 configuration.

## Verification

```text
show ip route 8.8.8.8
show ip ospf database external
```
