---
tags:
  - aws
  - saa-c03
  - performant
  - cost
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# AWS Elastic Beanstalk

## What it is for

Developer-centric PaaS that runs your code on EC2, ASG, ELB and RDS, managing capacity, load balancing, scaling, monitoring. You keep control of the underlying resources.

## Key concepts

- **Free**, you pay for the resources it creates.
- Components: an *application* holds *application versions* and *environments* (dev, test, prod).
- Tiers: *web server tier* behind an ELB; *worker tier* that processes messages from an SQS queue and scales on queue depth. The web tier can push work to the worker tier.
- Deployment modes: *single instance* (dev); *high availability with load balancer* (production).
- Platforms: Go, Java, .NET, Node.js, PHP, Python, Ruby, Packer, Docker (single and multi-container).
- Launching apps quickly: golden AMI + user data (that is what Beanstalk does).

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Free; you pay for the resources it creates.

## When to use it vs alternatives

- Deploy code without managing infrastructure but keeping control -> Beanstalk; explicit infrastructure as code -> [[cloudformation]].

## Exam traps

- Beanstalk is free.
- Worker tier = SQS.

## See also

[[cloudformation]], [[sqs]], [[elb]], [[auto-scaling]]
