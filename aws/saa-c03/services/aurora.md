---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
status: to-review
source: notion
notion-module: "05. Relational databases and caching"
---

# Amazon Aurora

## What it is for

AWS engine compatible with MySQL/PostgreSQL: more performance and flexibility than RDS.

## Key concepts

- Up to 5x the performance of MySQL on RDS and 3x that of PostgreSQL on RDS (AWS claim). Costs about 20% more than RDS but is more efficient.
- Storage grows automatically in 10 GB steps up to 256 TiB. **Six copies across three AZs**: writes need 4 of 6, reads 3 of 6; self-healing; striped across hundreds of volumes.
- One writer + up to **15 replicas**, failover in under 30 s, cross-region replicas and auto scaling.
- **Endpoints**: *writer/cluster* (always the primary), *reader* (load-balances across replicas), *custom* (a subset, e.g. larger replicas for analytics).
- **Aurora Serverless v2**: capacity scales automatically with load, billed per second; infrequent, intermittent or unpredictable workloads.
- **Global Database**: one primary region (R/W) + up to **10** read-only secondary regions, up to 16 replicas per secondary region, lag typically under a second, **RTO under a minute** when promoting a secondary for DR; also cuts read latency for distant users.
- **Cloning**: new cluster by copy-on-write, faster than snapshot/restore; staging from production.
- **Backtrack** (Aurora MySQL): rewind to a point in time without restoring.
- **Machine learning**: call SageMaker AI or Comprehend from SQL. **Babelfish**: Aurora PostgreSQL understands T-SQL (SQL Server migrations). **I/O-Optimised**: no per-I/O charges.
- Backups: retention 1 to 35 days, **cannot be disabled**, point-in-time recovery. External MySQL migration: Percona XtraBackup -> S3.

## Limits and quotas

- Storage up to 256 TiB; up to 15 replicas; Global Database: 10 secondary regions, 16 replicas per region.
- Backups: 1 to 35 days, cannot be disabled.

## Pricing (logic)

- ~20% more expensive than RDS but more efficient.
- Serverless v2: billed per second.
- I/O-Optimised removes per-I/O charges (useful for very I/O-heavy workloads).

## When to use it vs alternatives

- Need more performance or features than [[rds]] -> Aurora. Cross-region with RPO ~1 s and RTO < 1 min -> Global Database. Sporadic usage -> Serverless v2.

## Exam traps

- Cross-region DR with RPO around a second and RTO under a minute = Aurora Global Database.
- Fast staging copy from prod = cloning, not snapshot/restore.

## See also

[[rds]], [[dms-and-migration]], [[dr-and-multi-region]], [[dynamodb]]
