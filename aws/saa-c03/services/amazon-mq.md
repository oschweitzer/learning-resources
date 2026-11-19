---
tags:
  - aws
  - saa-c03
  - resilient
status: to-review
source: notion
notion-module: "10. Decoupling"
---

# Amazon MQ

## What it is for

Managed message broker for RabbitMQ and ActiveMQ (open protocols: MQTT, AMQP, STOMP, OpenWire, WSS).

## Key concepts

- Use it when migrating an on-premises application that already uses these protocols, instead of re-engineering it for SQS and SNS.
- **Not serverless**: runs on servers, does not scale as SQS and SNS do. Offers queues and topics.
- High availability: an active and a standby broker in two AZs sharing storage (EFS for ActiveMQ).

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Existing AMQP/MQTT app -> Amazon MQ; new cloud-native app -> [[sqs]] / [[sns]].

## Exam traps

- Existing AMQP or MQTT apps = Amazon MQ.

## See also

[[sqs]], [[sns]], [[sqs-vs-sns-vs-kinesis]]
