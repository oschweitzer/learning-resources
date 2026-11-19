---
tags:
  - aws
  - saa-c03
  - performant
  - secure
status: to-review
source: notion
notion-module: "14. Data and analytics"
---

# AWS Glue and AWS Lake Formation

## What it is for

Serverless ETL to prepare data (Glue) and build/govern a data lake with fine-grained permissions (Lake Formation).

## Key concepts

- **Glue**: serverless ETL. Classic pattern: S3 event -> Lambda/EventBridge -> Glue ETL job converting CSV to Parquet -> Athena queries.
- **Glue Data Catalog**: crawlers scan S3, RDS, DynamoDB, JDBC sources and write table metadata; used by Athena, Redshift Spectrum, EMR. **Job bookmarks** (avoid reprocessing), **DataBrew** (cleaning/normalising), **Glue Studio** (visual ETL), streaming ETL (Spark Structured Streaming: Kinesis, Kafka, MSK).
- **Lake Formation**: builds a data lake (a central place for all analytics data) in days instead of months, on top of S3 and the Glue Data Catalog; discovers, cleans, transforms and ingests data (blueprints); structured and unstructured data.
- Centralised access control, **fine-grained at row and column level**, enforced for Athena, Redshift, EMR, Quick...; cross-account data sharing.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Convert/transform -> Glue; row/column security on a data lake -> Lake Formation.

## Exam traps

- Row and column security on a data lake = Lake Formation.

## See also

[[athena]], [[redshift]], [[s3]], [[opensearch-emr-and-quick]]
