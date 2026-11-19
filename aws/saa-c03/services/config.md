---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "16. Monitoring and audit"
---

# AWS Config

## What it is for

Record the configuration of resources and its changes over time; check compliance. Answers 'what is the configuration, how did it change, does it comply?'.

## Key concepts

- Per region (aggregatable across regions/accounts), snapshots in S3. For each resource: compliance over time, configuration over time, the CloudTrail API calls that changed it. SNS notifications on changes.
- Questions: is there unrestricted SSH in my SGs? do my buckets have public access? how did this ALB change?
- **Config rules**: managed or custom (Lambda, Guard); evaluated on every configuration change and/or periodically. E.g. is every EBS volume gp3? allowed instance types? certificate about to expire (`acm-certificate-expiration-check`)? **They do not prevent anything** (no deny) -> to block use SCPs or IAM.
- **Remediation** through SSM Automation documents (managed or custom, may call Lambda), with retries; e.g. revoke IAM access keys older than 90 days. Notifications through EventBridge on non-compliance.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Billed per configuration item recorded and per rule evaluation.

## When to use it vs alternatives

- Compliance/configuration history -> Config; blocking -> [[organizations-and-scp]]. Comparison -> [[cloudwatch-vs-cloudtrail-vs-config]].

## Exam traps

- Config rules never block actions.
- Is it compliant? = Config.

## See also

[[cloudtrail]], [[cloudwatch]], [[systems-manager]], [[control-tower]], [[cloudwatch-vs-cloudtrail-vs-config]]
