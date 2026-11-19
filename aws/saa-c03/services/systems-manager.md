---
tags:
  - aws
  - saa-c03
  - secure
  - performant
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# AWS Systems Manager

## What it is for

Manage and access instances (EC2 and on premises) without SSH.

## Key concepts

- **Session Manager**: secure shell without SSH keys, bastion or port 22; Linux, macOS, Windows; session logs to S3/CloudWatch Logs; IAM-controlled.
- **Run Command**: run a document/command on many instances, no SSH; output to console, S3, CloudWatch; SNS notifications; can be triggered by EventBridge.
- **Patch Manager**: patches OS and applications (EC2 and on premises), on demand or scheduled, compliance reports. **Maintenance Windows**: schedule actions with a duration, instances and tasks.
- **Automation**: runbooks (SSM documents): restart instances, create AMIs, snapshot EBS; manual, EventBridge, scheduled, or Config remediation. **Parameter Store**: see [[secrets-manager-and-parameter-store]].
- Everything needs the **SSM agent** and an IAM role (instance profile) allowing Systems Manager.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Access without an open port -> Session Manager; scheduled patching -> Patch Manager + Maintenance Window.

## Exam traps

- Shell without opening port 22 = Session Manager.
- Patching 500 instances without SSH = Patch Manager + Maintenance Window.

## See also

[[ec2]], [[config]], [[eventbridge]], [[secrets-manager-and-parameter-store]]
