---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
status: to-review
source: notion
notion-module: "10. Decoupling"
---

# SQS vs SNS vs Kinesis Data Streams

Queue, pub/sub or stream: which decoupling model.

|  | SQS | SNS | Kinesis Data Streams |
| --- | --- | --- | --- |
| Model | Consumers pull | Pushed to many subscribers | Pull (standard) or push (enhanced fan-out) |
| Persistence | Until deleted (14 d max) | None, lost if not delivered | Up to 365 d, replayable |
| Consumers | Workers sharing one queue | Up to 12.5 M subscriptions per topic | Several applications reading the same data |
| Ordering | FIFO queues only | FIFO topics only | Per shard |
| Throughput | No provisioning (standard) | No provisioning | Provisioned shards or on-demand |
| Typical use | Decouple and buffer | Notify and fan out | Real-time analytics, ordered streams, replay |

## Decision rule

- Work buffer -> SQS.
- One event -> several consumers -> SNS + SQS fan-out.
- Replay / multiple consumers of a stream -> Kinesis.
- Existing AMQP/MQTT -> Amazon MQ.

## Exam traps

- Duplicate processing: visibility timeout + idempotency.

## See also

[[sqs]], [[sns]], [[kinesis-and-firehose]], [[amazon-mq]]
