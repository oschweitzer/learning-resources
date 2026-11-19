---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
  - resilient
status: to-review
source: notion
notion-module: "16. Monitoring and audit"
---

# CloudWatch vs CloudTrail vs Config

Which tool answers which question.

| Tool | Answers | For a load balancer |
| --- | --- | --- |
| CloudWatch | How is it performing? Alert me. Aggregate logs | Incoming connections, error codes over time, a performance dashboard |
| CloudTrail | Who called which API, when, from where? | See who changed the load balancer through API calls |
| Config | What is the configuration, how did it change, does it comply? | Track security group rules and configuration changes, ensure a TLS certificate is always attached |

## Decision rule

- Who did what -> CloudTrail.
- Is it compliant -> Config.
- Is it healthy -> CloudWatch.

## Exam traps

- Config rules do not block.
- CloudTrail event history = 90 days.

## See also

[[cloudwatch]], [[cloudtrail]], [[config]]
