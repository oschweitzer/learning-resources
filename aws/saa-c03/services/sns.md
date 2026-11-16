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

# Amazon SNS

## What it is for

Pub/sub: a producer publishes once to a topic, every subscriber receives the message.

## Key concepts

- Up to 12,500,000 subscriptions per topic and 100,000 topics per account.
- Subscribers: SQS, Lambda, Kinesis Data Firehose, HTTP(S) endpoints, email, SMS, mobile push (APNS, FCM, ADM). Many AWS services publish to SNS (CloudWatch alarms, ASG notifications, S3 events, CloudFormation).
- Security is the same as SQS (HTTPS, KMS, IAM, access policies).
- **Message filtering**: JSON filter policy on a subscription; without a filter policy, a subscriber gets everything.
- **Fan-out SNS + SQS**: publish once, deliver to several queues; fully decoupled, no data loss, each queue at its own pace, new queues can be added later. The SQS access policy must allow the topic; works cross-region. S3 case: only one event rule per event type and prefix -> send to SNS then fan out. SNS can also deliver to Firehose, then S3.
- **FIFO topics**: ordering by message group ID + deduplication; subscribers are SQS standard or FIFO queues; SNS FIFO + SQS FIFO = ordered, deduplicated fan-out.

## Limits and quotas

- 12.5 M subscriptions per topic; 100,000 topics per account.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Notify/fan-out -> SNS; workers that consume -> [[sqs]]; persistent stream -> [[kinesis-and-firehose]]. Comparison -> [[sqs-vs-sns-vs-kinesis]].

## Exam traps

- One event to several consumers = SNS fan-out to SQS.
- SNS does not persist: an undelivered message is lost.

## See also

[[sqs]], [[sqs-vs-sns-vs-kinesis]], [[lambda]], [[s3]], [[eventbridge]]
