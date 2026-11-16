---
tags:
  - aws
  - saa-c03
  - cost
  - resilient
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# AWS CloudFormation

## What it is for

Declarative infrastructure as code: describe resources, CloudFormation creates them in the right order.

## Key concepts

- Benefits: control and code review of all changes, resources tagged per stack (cost tracking), cost estimates from templates, scheduled deletion and recreation (dev environments at night), automatic diagrams, reuse of existing templates.
- **Infrastructure Composer** visualises templates and relations between resources. **Service role**: CloudFormation creates/updates/deletes on the user's behalf; users only need `iam:PassRole` (least privilege). **StackSets**: stacks across accounts and regions.
- CDK is out of scope for the exam, but it synthesises CloudFormation templates.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Cost estimates possible from templates; tags per stack for cost allocation.

## When to use it vs alternatives

- IaC to recreate an environment quickly (DR); Beanstalk is more guided ([[elastic-beanstalk]]).

## Exam traps

- Deploy without direct permissions on resources = service role + iam:PassRole.

## See also

[[control-tower]], [[dr-and-multi-region]], [[elastic-beanstalk]]
