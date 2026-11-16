---
tags:
  - aws
  - saa-c03
  - resilient
status: to-review
source: notion
notion-module: "16. Monitoring and audit"
---

# Amazon EventBridge

## What it is for

React to events (event patterns) and schedule actions (schedules). Formerly CloudWatch Events.

## Key concepts

- Sources: almost every AWS service (EC2 state changes, S3 uploads, GuardDuty findings, CodeBuild failures, Trusted Advisor), SaaS partners, your own apps. Targets: Lambda, SQS, SNS, Kinesis, Step Functions, ECS tasks, Batch, EC2 actions, API destinations...
- **Event buses**: default (AWS events), partner (SaaS), custom (your apps); shareable across accounts with resource-based policies (aggregate an organisation's events).
- **Archive and replay** (all or filtered, for a chosen period). **Schema registry**: infers schemas, generates code bindings, versioning.
- **Security**: Lambda/SNS/SQS/S3-related targets -> resource-based policy on the target; ECS/Kinesis/ASG-action targets -> IAM role.
- **EventBridge Scheduler** and **Pipes**: one-off/recurring schedules and point-to-point integrations with filtering and enrichment.
- With CloudTrail: intercepts any API call (e.g. deleting a DynamoDB table) and alerts via SNS. Example: root user sign-in -> SNS email.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- React to an event or schedule -> EventBridge; notify several subscribers -> [[sns]]; S3 to EventBridge for advanced filtering and replay.

## Exam traps

- Notify security on root sign-in = EventBridge rule + SNS.

## See also

[[cloudtrail]], [[sns]], [[lambda]], [[ecs-fargate-ecr]], [[s3]]
