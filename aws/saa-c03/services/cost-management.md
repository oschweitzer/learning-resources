---
tags:
  - aws
  - saa-c03
  - cost
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# Cost management tools

## What it is for

Visualise, alert, optimise AWS spend (domain 4).

## Key concepts

- **Cost Explorer**: visualise costs and usage, multi-account, monthly/hourly/per resource; recommends Savings Plans; forecasts.
- **Cost Anomaly Detection**: ML that learns your spending patterns and alerts on one-time spikes or continuous increases, **without thresholds**; monitors services, accounts, cost allocation tags, cost categories.
- **AWS Budgets**: alerts when costs/usage exceed, or are forecast to exceed, a budget; can trigger actions.
- **Cost and Usage Report** (Data Exports): the most detailed billing data, delivered to S3 for Athena/Quick. **Cost allocation tags**: split costs by project/team/environment.
- **Compute Optimizer**: right-sizing for EC2, ASG, EBS, Lambda, ECS on Fargate. **Savings Plans / RIs**: commitments ([[ec2-purchasing-options]]).
- **Instance Scheduler on AWS**: a CloudFormation-deployed solution that stops/starts EC2, ASG and RDS on schedules (DynamoDB, tags, Lambda); e.g. stop dev instances outside business hours.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Visualise -> Cost Explorer; alert on budget -> Budgets; anomalies without thresholds -> Cost Anomaly Detection; right-sizing -> Compute Optimizer.

## Exam traps

- Unexpected spend without thresholds = Cost Anomaly Detection.
- Right-sizing = Compute Optimizer.

## See also

[[ec2-purchasing-options]], [[organizations-and-scp]], [[athena]], [[well-architected-and-trusted-advisor]]
