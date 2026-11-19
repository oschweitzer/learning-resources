---
tags:
  - aws
  - saa-c03
  - resilient
status: to-review
source: notion
notion-module: "12. Serverless"
---

# AWS Step Functions

## What it is for

Serverless visual workflows that orchestrate Lambda and other services (ECS, Batch, DynamoDB, SQS, on-premises activities).

## Key concepts

- Sequences, parallel branches, choices, retries, timeouts, error handling, waits, human approval steps.
- **Standard**: up to a year, exactly-once execution. **Express**: cheaper, high-volume, short (up to 5 minutes) event processing.
- Use cases: order fulfilment, data processing pipelines, ML workflows, web application flows.

## Limits and quotas

- Standard: up to 1 year. Express: up to 5 minutes.

## Pricing (logic)

- Express: cheaper, for high-volume workloads.

## When to use it vs alternatives

- Orchestrate several steps/services -> Step Functions; split a job that exceeds [[lambda]]'s 15 minutes.

## Exam traps

- Standard = exactly-once up to 1 year; Express = high volume and short.

## See also

[[lambda]], [[api-gateway]]
