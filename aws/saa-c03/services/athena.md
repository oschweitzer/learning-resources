---
tags:
  - aws
  - saa-c03
  - performant
  - cost
status: to-review
source: notion
notion-module: "14. Data and analytics"
---

# Amazon Athena

## What it is for

Serverless SQL queries directly on files in S3 (CSV, JSON, ORC, Avro, Parquet), built on Trino/Presto.

## Key concepts

- Use cases: ad hoc queries, BI, reporting, analysing VPC Flow Logs, ELB logs, CloudTrail logs. Often paired with Amazon Quick for dashboards.
- **Cut cost / speed up**: columnar formats (Parquet, ORC; Glue can convert), compression (gzip, snappy, zstd...), **partitioning** (s3://bucket/year=2026/month=11/day=03/), fewer, larger files (> 128 MB).
- **Federated queries**: data source connectors (Lambda functions) to relational, NoSQL, object and custom sources on AWS or on premises (CloudWatch Logs, DynamoDB, RDS, DocumentDB, Redshift). Results stored in S3.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Billed per TB scanned (hence columnar formats, compression, partitions).

## When to use it vs alternatives

- Occasional queries on S3 without loading -> Athena; heavy repeated joins/aggregations -> [[redshift]]. Comparison -> [[athena-vs-redshift]].

## Exam traps

- SQL on S3 with no servers = Athena.
- Make Athena cheaper = Parquet, compression, partitions.

## See also

[[redshift]], [[athena-vs-redshift]], [[glue-and-lake-formation]], [[s3]], [[opensearch-emr-and-quick]]
