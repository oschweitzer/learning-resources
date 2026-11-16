---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
status: to-review
source: notion
notion-module: "14. Data and analytics"
---

# Managed Service for Apache Flink and Amazon MSK

## What it is for

Stream analytics (Flink) and managed Kafka (MSK), as an alternative/complement to Kinesis.

## Key concepts

- **Managed Service for Apache Flink** (formerly Kinesis Data Analytics for Flink): runs Flink applications (Java, Scala, SQL) on a managed, auto-scaling cluster, with checkpoints and snapshots. Reads from Kinesis Data Streams or MSK, **not Firehose**.
- **Amazon MSK**: managed Apache Kafka (brokers in your VPC across up to 3 AZs, failure recovery, data on EBS as long as you want). **MSK Serverless**: Kafka without managing capacity. Consumers: Flink, Glue streaming ETL, Lambda, apps on EC2/ECS/EKS.
- Reference pipeline: IoT Core -> Kinesis Data Streams -> Firehose (every minute or so) -> S3 -> SQS/Lambda -> Athena queries -> Quick, and Redshift for warehousing.

## Limits and quotas

- Kinesis: records up to 10 MiB, retention 365 d. MSK: 1 MB by default (configurable), retention as long as you pay for storage.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Existing/portable Kafka -> MSK; AWS-native stream -> [[kinesis-and-firehose]]. Comparison -> [[kinesis-vs-msk]].

## Exam traps

- Kafka = MSK.
- Flink cannot read Firehose.

## See also

[[kinesis-and-firehose]], [[kinesis-vs-msk]], [[athena]], [[redshift]]
