---
tags:
  - aws
  - saa-c03
  - performant
status: to-review
source: notion
notion-module: "14. Data and analytics"
---

# Amazon Redshift

## What it is for

PostgreSQL-based data warehouse for OLAP (not OLTP): columnar storage, massively parallel engine, petabytes.

## Key concepts

- Two modes: **provisioned** (node types, reservable) and **Serverless**. A cluster = leader node (planning, aggregation) + compute nodes.
- Faster than Athena for joins, aggregations and repeated complex queries (loaded data, indexes); Athena is better for occasional queries without loading.
- **Snapshots / DR**: Multi-AZ for some node types; incremental snapshots in S3; a restore creates a **new cluster**; automated every 8 hours, every 5 GB or on a schedule; manual snapshots kept; automatic copy to another region.
- **Loading**: Firehose -> S3 then `COPY`; `COPY` from S3 with an IAM role (ideally **enhanced VPC routing**); JDBC inserts possible but batch them.
- **Redshift Spectrum**: queries S3 without loading, from an existing Redshift cluster, pushing work to thousands of Spectrum nodes.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Provisioned: reserve to save; Serverless available.

## When to use it vs alternatives

- Heavy repeated analytics -> Redshift. Query S3 from Redshift without loading -> Spectrum. Occasional on S3 -> [[athena]].

## Exam traps

- Complex repeated analytics = Redshift.
- Query S3 from Redshift without loading = Spectrum.

## See also

[[athena]], [[athena-vs-redshift]], [[kinesis-and-firehose]], [[s3]], [[glue-and-lake-formation]]
