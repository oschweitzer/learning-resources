---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
status: to-review
source: notion
notion-module: "05. Relational databases and caching"
---

# Read replica vs Multi-AZ (RDS)

They are not interchangeable: one scales reads, the other provides availability.

|  | Read replicas | Multi-AZ (instance) |
| --- | --- | --- |
| Purpose | Scale reads | Availability and DR |
| Replication | Asynchronous, eventually consistent | Synchronous |
| Location | Same AZ, cross-AZ or cross-region | Standby in another AZ of the same region |
| Readable | Yes (own endpoint) | No (the standby only takes over) |
| Failover | Manual promotion | Automatic, same DNS |
| Count | Up to 15 | One standby |

## Decision rule

- Reporting/heavy analytics without slowing prod -> read replica.
- Survive the loss of an AZ -> Multi-AZ.
- Both -> a read replica set up as Multi-AZ.

## Exam traps

- The Multi-AZ standby is not readable (except Multi-AZ DB clusters).

## See also

[[rds]], [[aurora]]
