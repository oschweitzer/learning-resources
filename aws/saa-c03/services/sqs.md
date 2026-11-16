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

# Amazon SQS

## What it is for

Managed message queue to decouple layers and absorb spikes (e.g. 1,000 videos to encode instead of 10).

## Key concepts

- **Standard**: the oldest AWS service, unlimited throughput and messages, retention 4 days by default (14 max), latency under 10 ms. **At-least-once** delivery (duplicates possible), best-effort ordering -> **idempotent** consumers.
- Messages up to **1 MiB** (since August 2025; previously 256 KiB). For bigger payloads: payload in S3 + a pointer.
- Producer: `SendMessage`. Consumers (EC2, on-premises, Lambda) poll up to 10 messages, process, then `DeleteMessage`. Many consumers in parallel; scale an ASG on `ApproximateNumberOfMessagesVisible` (or backlog per instance).
- **Visibility timeout**: 30 s by default; if the message is not deleted in time it becomes visible again (double processing). `ChangeMessageVisibility` for more time. Too high: slow reprocessing after a crash; too low: duplicates.
- **Long polling** (1 to 20 s, `ReceiveMessageWaitTimeSeconds`): fewer empty responses, lower latency, always preferred over short polling. **Delay queue**: up to 15 min.
- **DLQ**: receives messages that failed more than `maxReceiveCount` times. A FIFO queue needs a FIFO DLQ. *Redrive* sends them back to the source queue once fixed.
- **FIFO**: strict ordering per *message group ID*, exactly-once via deduplication (ID or content-based, 5-minute window); 300 msg/s (3,000 with batching), high throughput mode raises this a lot; name ends in `.fifo`.
- Security: HTTPS, KMS at rest, optional client-side encryption; IAM + SQS access policies (cross-account, or to let SNS/S3 write to the queue).

## Limits and quotas

- Retention: 4 days (default), 14 days (max).
- Message: 1 MiB.
- Default visibility timeout: 30 s.
- Long polling: 1 to 20 s.
- Delay queue: 15 min max.
- FIFO: 300 msg/s, 3,000 with batching.
- FIFO deduplication window: 5 min.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Pull by workers -> SQS; notify several subscribers -> [[sns]]; replayable real-time stream -> [[kinesis-and-firehose]]; existing AMQP/MQTT protocols -> [[amazon-mq]]. Comparison -> [[sqs-vs-sns-vs-kinesis]].

## Exam traps

- Duplicate processing -> raise the visibility timeout, make consumers idempotent.
- Ordering + exactly-once = FIFO.
- One event to many consumers = SNS fan-out to SQS.

## See also

[[sns]], [[sqs-vs-sns-vs-kinesis]], [[lambda]], [[auto-scaling]], [[kinesis-and-firehose]]
