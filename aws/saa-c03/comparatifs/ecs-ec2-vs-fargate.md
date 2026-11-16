---
tags:
  - aws
  - saa-c03
  - comparatif
  - cost
  - performant
status: to-review
source: notion
notion-module: "11. Containers"
---

# ECS on EC2 vs Fargate vs Managed Instances

Choose the ECS capacity mode.

|  | EC2 launch type | Fargate | ECS Managed Instances |
| --- | --- | --- | --- |
| Infrastructure | You manage EC2 instances running the ECS agent | None | AWS provisions and patches EC2 |
| Scaling | Tasks + instances (ASG, capacity provider) | Number of tasks only | Managed by AWS |
| Control | Full (types, GPUs, daemons) | Least | More instance choice than Fargate |
| Typical answer | Specific hardware or cost at high density | Serverless, least operational effort | Managed EC2 without running the fleet |

## Decision rule

- Less operations -> Fargate.
- GPU / specific hardware -> EC2.
- Managed EC2 -> Managed Instances.

## Exam traps

- S3 Files: Fargate and Managed Instances, not the EC2 launch type.

## See also

[[ecs-fargate-ecr]], [[eks]]
