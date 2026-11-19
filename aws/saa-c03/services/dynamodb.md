---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - cost
status: to-review
source: notion
notion-module: "12. Serverless"
---

# Amazon DynamoDB (and DAX)

## What it is for

Serverless key-value/document NoSQL database replicated across 3 AZs, single-digit millisecond latency, millions of requests per second.

## Key concepts

- Primary key chosen at creation: *partition key* or partition key + *sort key*. Items up to 400 KB. Table classes: Standard and Standard-IA (cheaper storage for rarely read tables).
- **Capacity**: *Provisioned* (RCU/WCU, optional auto scaling; predictable traffic, cost control); *On-demand* (instant scaling, pay per request, more expensive per request; unpredictable traffic, new apps).
- **DAX**: in-memory cache for DynamoDB, microsecond reads, no application logic change; default TTL 5 min; caches items and query results (ElastiCache fits computed/aggregated results).
- **DynamoDB Streams**: ordered stream of item-level changes, kept 24 h, read by Lambda or KCL. **Kinesis Data Streams for DynamoDB**: up to 1 year, more consumers. Use cases: welcome emails, real-time analytics, derived tables, OpenSearch indexing.
- **Global tables**: multi-region active-active replication (reads/writes anywhere), Streams required. Default: multi-region eventual consistency (usually 1 to 2 s). **Multi-region strong consistency** (GA June 2025, chosen at creation): **RPO zero**.
- **TTL**: automatic deletion after an expiry timestamp (sessions, regulatory retention). DynamoDB can replace ElastiCache as a session store thanks to TTL.
- **PITR**: restore to any second within up to 35 days, into a new table. On-demand backups last until deleted (manageable by AWS Backup with cross-region copies).
- **Export to S3** (needs PITR, consumes no read capacity) for Athena/audits; **import from S3** (CSV, DynamoDB JSON, Ion) creates a new table without consuming write capacity.

## Limits and quotas

- Item up to 400 KB.
- Streams: 24 h retention.
- PITR: up to 35 days.
- DAX: default TTL 5 min.

## Pricing (logic)

- Provisioned: capacity in RCU/WCU; on-demand: pay per request (more expensive per request). Standard-IA for cheaper storage.
- Export to S3 and import from S3 consume no capacity.

## When to use it vs alternatives

- Microsecond cache -> DAX. Full-text search -> DynamoDB Streams -> Lambda -> [[opensearch-emr-and-quick]]. Relational/joins -> [[rds]] / [[aurora]]. Sessions -> ElastiCache or DynamoDB + TTL ([[elasticache]]).

## Exam traps

- Unpredictable spikes = on-demand.
- Microseconds without code changes = DAX.
- Zero data loss across regions = global tables with strong consistency.

## See also

[[elasticache]], [[lambda]], [[kms]], [[aws-backup]], [[specialized-databases]], [[api-gateway]], [[dr-and-multi-region]], [[opensearch-emr-and-quick]]
