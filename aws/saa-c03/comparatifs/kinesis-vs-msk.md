---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
status: to-review
source: notion
notion-module: "14. Data and analytics"
---

# Kinesis Data Streams vs Amazon MSK

AWS-native or Kafka.

| Kinesis Data Streams | Amazon MSK |
| --- | --- |
| Records up to 10 MiB, typically small | 1 MB by default, configurable |
| Streams with shards (split/merge) | Topics with partitions (add only) |
| TLS in flight | Plaintext or TLS |
| KMS at rest | KMS at rest |
| Retention up to 365 d | Retention as long as the storage you pay for |
| AWS API | Kafka API, portable |

## Decision rule

- Need the Kafka API / portability -> MSK.
- Simple AWS-native integration -> Kinesis.

## Exam traps

- Nothing specific in my Notion notes.

## See also

[[kinesis-and-firehose]], [[streaming-analytics]]
