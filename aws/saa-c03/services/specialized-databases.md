---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
status: to-review
source: notion
notion-module: "13. Choosing a database"
---

# Specialised databases: DocumentDB, Neptune, Keyspaces, Timestream, MemoryDB

## What it is for

Match a workload to the right purpose-built database (relational, key-value, document, graph, time series, wide column, in-memory).

## Key concepts

- Questions to ask: read-heavy, write-heavy or balanced? Throughput variation? Data volume and growth? Durability, source of truth? Latency, concurrency? Data model and queries? Licence costs (move to Aurora?).
- Map: relational (RDS, Aurora); key-value/document (DynamoDB, DocumentDB); in-memory (ElastiCache, MemoryDB, DAX); wide column (Keyspaces); graph (Neptune); time series (Timestream); search (OpenSearch); data warehouse (Redshift, Athena, EMR); object store (S3, Glacier).
- **DocumentDB**: what Aurora is to MySQL/PostgreSQL, DocumentDB is to **MongoDB**; JSON; 3 AZs; storage grows in 10 GB steps; millions of requests per second; elastic clusters (write sharding).
- **Neptune**: graph database for highly connected data (social networks, knowledge graphs, fraud detection, recommendations); 3 AZs, up to 15 read replicas; billions of relationships with millisecond queries; **Neptune Streams** (ordered changes, no duplicates, via an HTTP REST API).
- **Keyspaces**: managed serverless Apache Cassandra-compatible (CQL), tables replicated three times across AZs, single-digit ms; on-demand or provisioned + auto scaling; PITR up to 35 days; IoT, time series, Cassandra migration.
- **Timestream**: time series (SQL, scheduled queries, multi-measure records); recent data in memory, history in a cheaper magnetic tier; ingests from IoT, Kinesis, MSK, Prometheus, Telegraf, Lambda. **Status**: Timestream for LiveAnalytics closed to new customers since 20 June 2025 -> Timestream for InfluxDB recommended; for the exam 'time series database' still means Timestream.
- Summaries: RDS (SQL/OLTP), Aurora (RDS with more), ElastiCache (cache, sessions, leaderboards), DynamoDB (serverless, known access patterns), S3 (big objects, not many tiny ones).

## Limits and quotas

- DocumentDB: storage in 10 GB steps; Neptune: up to 15 read replicas; Keyspaces: PITR up to 35 days.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- MongoDB -> DocumentDB; Cassandra -> Keyspaces; graph -> Neptune; time series -> Timestream; search on any field -> [[opensearch-emr-and-quick]]; petabyte analytics -> [[redshift]].

## Exam traps

- MongoDB = DocumentDB.
- Cassandra = Keyspaces.
- Relationships/graph traversal = Neptune.
- Search on any field = OpenSearch, often fed by DynamoDB Streams.
- Petabyte analytics = Redshift, not RDS.

## See also

[[dynamodb]], [[rds]], [[aurora]], [[elasticache]], [[redshift]], [[opensearch-emr-and-quick]]
