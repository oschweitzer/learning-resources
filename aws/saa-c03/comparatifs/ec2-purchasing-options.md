---
tags:
  - aws
  - saa-c03
  - comparatif
  - cost
status: to-review
source: notion
notion-module: "02. EC2 fundamentals"
---

# EC2 purchasing options

Pick the right purchasing model for duration, flexibility and tolerance to interruption.

| Option | Discount vs On-Demand | Commitment | Fits |
| --- | --- | --- | --- |
| On-Demand | None | None | Short, unpredictable, uninterrupted workloads |
| Standard Reserved Instances | Up to 72% | 1 or 3 years (type, region, tenancy, OS) | Steady state (e.g. a database) |
| Convertible Reserved Instances | Up to 66% | 1 or 3 years, can change type/OS/family | Steady state that may evolve |
| Compute Savings Plans | Up to 66% | Spend per hour 1 or 3 years, any family/region/OS, also Fargate and Lambda | Long-term usage with flexibility |
| EC2 Instance Savings Plans | Up to 72% | Spend per hour, one family in a region | Long-term usage of one family |
| Spot | Up to 90% | None, but reclaimed with a 2-minute notice | Fault-tolerant jobs; never a critical database |
| Dedicated Hosts | Varies | On-Demand or reserved | Licences per socket/core, strict compliance; the most expensive |
| Dedicated Instances | Varies | None | Hardware not shared, no control over placement |
| On-Demand Capacity Reservations | None | None, billed whether used or not | Guarantee capacity in one AZ; combine with Savings Plans/regional RIs |

## Decision rule

- Interruptible job -> Spot (price-capacity-optimized fleet).
- Steady state -> RIs / Savings Plans; need flexibility -> Compute Savings Plans.
- Per-core licences -> Dedicated Host.
- Guaranteed capacity without a discount -> Capacity Reservation.

## Exam traps

- Capacity Reservation = capacity, no discount.
- Spot never for a critical database.

## See also

[[ec2]], [[cost-management]]
