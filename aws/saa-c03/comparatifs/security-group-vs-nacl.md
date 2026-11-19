---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# Security group vs NACL

Two levels of filtering.

| Security group | NACL |
| --- | --- |
| Instance (ENI) level | Subnet level |
| Allow rules only | Allow and deny rules |
| Stateful: return traffic automatic | Stateless: return traffic must be allowed |
| All rules evaluated together | Evaluated in order, first match wins |
| Applies to the instances it is attached to | Applies to every instance in the subnet |

## Decision rule

- Block an IP -> NACL.
- Allow only the LB -> SG referencing the LB's SG.

## Exam traps

- NACL: ephemeral ports.
- SGs cannot deny.

## See also

[[security-groups-and-nacl]]
