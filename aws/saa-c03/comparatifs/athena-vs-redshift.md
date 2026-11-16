---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
status: to-review
source: notion
notion-module: "14. Data and analytics"
---

# Athena vs Redshift

Query S3 on demand or warehouse for heavy analytics.

|  | Athena | Redshift |
| --- | --- | --- |
| Model | Serverless, SQL on S3 files, no loading | Data warehouse (PostgreSQL), columnar storage, MPP, loaded data |
| Billing | Per TB scanned | Provisioned cluster or Serverless |
| Ideal for | Ad hoc queries, logs (Flow Logs, ELB, CloudTrail) | Joins, aggregations, repeated complex queries |
| Bridge | - | Spectrum queries S3 without loading |

## Decision rule

- Occasional on S3 -> Athena.
- Heavy repeated analytics -> Redshift.

## Exam traps

- Athena: Parquet + compression + partitions to cut cost.

## See also

[[athena]], [[redshift]]
