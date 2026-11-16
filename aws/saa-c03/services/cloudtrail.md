---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "16. Monitoring and audit"
---

# AWS CloudTrail

## What it is for

Audit and governance: records API calls made in the account (console, SDK, CLI, AWS services). Answers 'who did what, when, from where?'.

## Key concepts

- **Enabled by default**. *Event history* keeps management events for **90 days**; a **trail** sends events to S3 (and optionally CloudWatch Logs) for long-term retention, then Athena. A trail can cover all regions (recommended) or one; an **organisation trail** covers every account.
- **Event types**: *management* (IAM AttachRolePolicy, EC2 CreateSubnet...; logged by default; read/write); *data events* (S3 GetObject/PutObject/DeleteObject, Lambda Invoke; **not** by default, high volume); *Insights* (unusual activity; not by default, paid; baseline on write management events, to console, S3, EventBridge).
- **CloudTrail Lake**: store and query events with SQL over a longer period.
- Typical question: someone deleted a resource, who was it? -> CloudTrail.

## Limits and quotas

- Event history: 90 days.

## Pricing (logic)

- Data events and Insights are optional/paid (high volume).

## When to use it vs alternatives

- Who did what -> CloudTrail; performance -> [[cloudwatch]]; compliance -> [[config]]. Comparison -> [[cloudwatch-vs-cloudtrail-vs-config]].

## Exam traps

- CloudTrail only keeps 90 days unless you create a trail.
- S3 object-level calls = data events, to be enabled.
- Who deleted it = CloudTrail.

## See also

[[cloudwatch]], [[config]], [[eventbridge]], [[athena]], [[cloudwatch-vs-cloudtrail-vs-config]], [[organizations-and-scp]]
