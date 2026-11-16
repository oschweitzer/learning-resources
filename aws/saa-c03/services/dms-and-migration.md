---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
status: to-review
source: notion
notion-module: "20. Disaster recovery, migration and multi-region"
---

# AWS DMS, SCT and migrating to Aurora

## What it is for

Migrate databases to AWS quickly and securely while the source stays available.

## Key concepts

- **DMS**: homogeneous (Oracle to Oracle) and heterogeneous (SQL Server to Aurora) migrations; **continuous replication with CDC**; runs on a *replication instance* (or DMS Serverless).
- Sources: on-premises/EC2 databases (Oracle, SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, Db2), Azure SQL Database, RDS/Aurora, S3, DocumentDB. Targets: on-premises/EC2 databases, RDS, Aurora, Redshift, DynamoDB, S3, OpenSearch, Kinesis Data Streams, Kafka, DocumentDB, Neptune, Redis.
- **SCT** (Schema Conversion Tool): converts schemas between engines, OLTP (SQL Server/Oracle -> MySQL/PostgreSQL/Aurora) and OLAP (Teradata/Oracle -> Redshift); **not needed with the same engine**; prefer compute-optimised instances for conversion. Pattern: SCT on premises converts the schema, then DMS (replication instance in AWS) runs full load + CDC.
- **Multi-AZ DMS**: synchronous standby replica in another AZ (redundancy, fewer I/O freezes and latency spikes).
- **To Aurora**: from RDS MySQL/PostgreSQL -> restore an RDS snapshot as an Aurora cluster (downtime) or create an Aurora read replica of the RDS instance and promote it when lag is zero (time and cost); external MySQL -> Percona XtraBackup to S3 then create an Aurora MySQL cluster from S3 (or mysqldump, slower); external PostgreSQL -> backup to S3 then import with the `aws_s3` extension; both running -> DMS with continuous replication.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Heterogeneous migration -> SCT + DMS; same engine -> DMS (or native tools). Other tools -> [[migration-and-tools]].

## Exam traps

- Heterogeneous = SCT + DMS.
- SCT is not needed for the same engine.

## See also

[[aurora]], [[rds]], [[migration-and-tools]], [[dynamodb]], [[redshift]]
