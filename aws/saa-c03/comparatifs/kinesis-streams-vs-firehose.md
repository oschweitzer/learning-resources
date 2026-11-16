---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
status: to-review
source: notion
notion-module: "10. Decoupling"
---

# Kinesis Data Streams vs Amazon Data Firehose

Collect a stream or load it into a destination.

| Kinesis Data Streams | Amazon Data Firehose |
| --- | --- |
| Collects streams, you write producers and consumers | Loads streams into destinations, no code |
| Real time | Near real-time (buffering) |
| Provisioned or on-demand | Fully automatic scaling |
| Storage up to 365 d, replay possible | No storage and no replay |

## Decision rule

- Need replay or several consumers -> Data Streams.
- Load S3/Redshift/OpenSearch without code -> Firehose.

## Exam traps

- Flink cannot read Firehose.

## See also

[[kinesis-and-firehose]]
