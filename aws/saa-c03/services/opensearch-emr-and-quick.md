---
tags:
  - aws
  - saa-c03
  - performant
status: to-review
source: notion
notion-module: "14. Data and analytics"
---

# OpenSearch Service, EMR and Amazon Quick

## What it is for

Search/log analytics (OpenSearch), Hadoop/Spark big data (EMR) and serverless BI (Quick).

## Key concepts

- **OpenSearch Service** (successor of Elasticsearch Service): searches **any field**, including partial matches (DynamoDB only queries by key/index); complement to another database; log analytics with dashboards; managed clusters or serverless; ingests from Firehose, IoT, CloudWatch Logs; security via Cognito, IAM, KMS, TLS.
- OpenSearch patterns: DynamoDB -> Streams -> Lambda -> OpenSearch (search in OpenSearch, then fetch the full item from DynamoDB); CloudWatch Logs subscription filter -> Lambda (real time) or Firehose (near real-time) -> OpenSearch; Kinesis Data Streams -> Firehose or Lambda -> OpenSearch.
- **EMR**: managed Hadoop clusters (hundreds of EC2 instances) with Spark, HBase, Presto, Flink; provisioning, configuration, auto scaling, Spot integration. Also on EKS and serverless. **Nodes**: primary (cluster, long-running: On-Demand/Reserved), core (tasks + HDFS data, long-running: On-Demand/Reserved), task (tasks only, no storage: **Spot**). Clusters can be long-running or transient.
- **Amazon Quick** (formerly QuickSight): serverless ML-powered BI with interactive dashboards; priced per user or session. In October 2025 QuickSight became Amazon Quick Suite, shortened to Amazon Quick in 2026 (the BI part is Quick Sight); the exam guide says Amazon Quick. Sources: RDS, Aurora, Athena, Redshift, S3, OpenSearch, Timestream, SaaS (Salesforce, Jira), on premises via JDBC, files. **SPICE** = in-memory engine (imported data). Enterprise edition adds column-level security. Users and groups exist **only in Quick**, not IAM; a dashboard is a read-only snapshot of an analysis.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- EMR: Spot recommended for task nodes.

## When to use it vs alternatives

- Full-text search -> OpenSearch; Spark/Hadoop big data -> EMR; dashboards -> Quick.

## Exam traps

- Search on any field = OpenSearch.
- EMR task nodes = Spot.
- Quick users are not IAM users.

## See also

[[dynamodb]], [[athena]], [[redshift]], [[glue-and-lake-formation]], [[specialized-databases]]
