---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
status: to-review
source: notion
notion-module: "20. Disaster recovery, migration and multi-region"
---

# DR strategies: backup, pilot light, warm standby, multi-site

Cheapest to most expensive; RPO/RTO values are orders of magnitude.

| Strategy | Principle | Typical RPO / RTO | Cost |
| --- | --- | --- | --- |
| Backup and restore | Backups copied (snapshots, AMIs, S3...), infrastructure rebuilt after the incident | Hours | Lowest |
| Pilot light | Critical core running (replicated database), app servers ready but off | Tens of minutes | Low |
| Warm standby | Full copy at reduced scale, scaled up during an incident | Minutes | Medium |
| Multi-site (active-active) | Full scale in two places both serving traffic | Close to zero, seconds | Highest |

## Decision rule

- Choose per application from RTO/RPO and budget.
- The shorter, the more expensive.

## Exam traps

- An untested DR plan is non-existent.
- Plan quotas in the standby region.

## See also

[[dr-and-multi-region]], [[aws-backup]]
