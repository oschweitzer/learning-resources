---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - secure
status: to-review
source: notion
notion-module: "05. Relational databases and caching"
---

# Amazon RDS (and RDS Proxy)

## What it is for

Managed relational SQL databases: provisioning, OS patching, backups, monitoring, read replicas, Multi-AZ, maintenance windows.

## Key concepts

- Engines: PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, IBM Db2, Aurora. Storage on EBS. No SSH (except **RDS Custom**).
- **Storage Auto Scaling**: grows storage up to a maximum you set when free space < 10% for at least 5 minutes and the last change was more than 6 hours ago.
- **Read replicas**: scale reads, asynchronous, same AZ / cross-AZ / cross-region, up to 15, own endpoint (the app must use it), SELECT only, manual promotion. A replica can itself be Multi-AZ.
- **Multi-AZ (instance)**: availability/DR, **synchronous** replication, one non-readable standby in another AZ, automatic failover, same DNS name. Single-AZ to Multi-AZ is a zero-downtime modify (snapshot -> restore -> replication).
- **Multi-AZ DB cluster** (MySQL, PostgreSQL): 1 writer + 2 readable standbys across 3 AZs, faster failover.
- Network: replication between AZs in the same region is free for RDS; cross-region replication is charged.
- **RDS Custom** (Oracle, SQL Server): access to the OS and database; pause automation before customising, snapshot first; SSH or Session Manager.
- **Backups**: automated = daily full backup + transaction logs every 5 minutes, point-in-time restore, retention 1 to 35 days (0 disables); manual snapshots kept as long as you want. A restore **always creates a new database**. A stopped database still pays for storage.
- **Security**: KMS encryption **at launch** (if the primary is not encrypted, replicas cannot be). To encrypt an existing database: snapshot -> encrypted copy -> restore. TLS in flight. IAM database authentication (MySQL, PostgreSQL). Credentials in Secrets Manager (rotation).
- **RDS Proxy**: fully managed, serverless, Multi-AZ proxy that pools connections; cuts failover time by up to 66%; IAM auth + Secrets Manager; **never public** (inside the VPC). Ideal for Lambda opening many short-lived connections.
- On-premises MySQL -> RDS MySQL: backup file in S3.

## Limits and quotas

- Read replicas: up to 15.
- Automated backup retention: 1 to 35 days.
- Storage Auto Scaling: < 10% free for 5 min, > 6 h since the last change.

## Pricing (logic)

- Replication between AZs in the region is free; cross-region is charged.
- A stopped database still pays for storage: for a database used a few hours a month, snapshot it and restore when needed.

## When to use it vs alternatives

- Read replica vs Multi-AZ -> [[read-replica-vs-multi-az]]. More performance / features -> [[aurora]]. Cache -> [[elasticache]]. NoSQL -> [[dynamodb]].
- Lambda + many connections -> RDS Proxy (the function must be in the VPC).

## Exam traps

- Read replicas scale reads, Multi-AZ gives availability; they are not interchangeable.
- The Multi-AZ standby is not readable (except Multi-AZ DB clusters).
- Encryption is decided at creation.
- Lambda + too many connections = RDS Proxy.

## See also

[[aurora]], [[read-replica-vs-multi-az]], [[elasticache]], [[kms]], [[secrets-manager-and-parameter-store]], [[lambda]], [[dms-and-migration]]
