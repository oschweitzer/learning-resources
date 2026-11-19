---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# Lambda vs AWS Batch

When a job no longer fits in Lambda.

| Lambda | AWS Batch |
| --- | --- |
| 15-minute limit | No time limit |
| Limited runtimes | Any runtime packaged as a Docker image |
| Limited temporary disk (/tmp up to 10 GB) | EBS or instance store for disk |
| Serverless | Relies on EC2 (managed by AWS) or Fargate |

## Decision rule

- Short, event-driven -> Lambda.
- Long, big disk, custom image -> Batch (or Fargate).

## Exam traps

- A job over 15 minutes cannot run on Lambda.

## See also

[[lambda]], [[other-services]], [[ecs-fargate-ecr]]
