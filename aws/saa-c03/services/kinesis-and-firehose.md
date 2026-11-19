---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
status: to-review
source: notion
notion-module: "10. Decoupling"
---

# Kinesis Data Streams and Amazon Data Firehose

## What it is for

Collect and process streaming data in real time (Data Streams) or load it into a destination (Firehose).

## Key concepts

- **Kinesis Data Streams**: clickstreams, IoT, logs, metrics. Retention up to **365 days**, replayable, data cannot be deleted before it expires. Records up to 10 MiB (typically small). Ordering guaranteed per **partition key** -> shard. KMS at rest, HTTPS in flight.
- Producers: SDK, KPL, Kinesis Agent. Consumers: KCL, SDK, Lambda, Firehose, Managed Service for Apache Flink.
- Capacity modes: *Provisioned* (shards; 1 MB/s or 1,000 records/s in, 2 MB/s out per shard; split/merge; billed per shard-hour) and *On-demand* (starts at 4 MB/s or 4,000 records/s in, scales on the peak of the last 30 days; billed per stream-hour + GB in/out).
- **Amazon Data Firehose** (formerly Kinesis Data Firehose): managed, serverless, auto-scaling. Loads S3, Redshift (via S3), OpenSearch, Splunk, Datadog, New Relic, MongoDB, custom HTTP. **Near real-time** (buffers by size/time). Lambda transformation, Parquet/ORC conversion, compression, backup of all or failed data to S3. Sources: Data Streams, direct PUT, CloudWatch Logs/Events, IoT, SNS.
- Data Streams vs Firehose: you write producer/consumer code vs no code; real time vs near real-time; provisioned/on-demand vs automatic scaling; storage up to 365 days + replay vs no storage and no replay.

## Limits and quotas

- Record up to 10 MiB.
- Retention up to 365 days.
- Provisioned shard: 1 MB/s or 1,000 records/s in, 2 MB/s out.
- On-demand: starts at 4 MB/s or 4,000 records/s.

## Pricing (logic)

- Data Streams provisioned: per shard-hour; on-demand: per stream-hour + GB in/out.
- Firehose: pay for what you use.

## When to use it vs alternatives

- Comparison -> [[kinesis-streams-vs-firehose]] and [[sqs-vs-sns-vs-kinesis]]. Kafka -> [[streaming-analytics]].

## Exam traps

- Replay or several consumers of the same real-time data = Data Streams.
- Load into S3/Redshift without code = Firehose.

## See also

[[kinesis-streams-vs-firehose]], [[sqs-vs-sns-vs-kinesis]], [[streaming-analytics]], [[s3]], [[redshift]], [[lambda]]
