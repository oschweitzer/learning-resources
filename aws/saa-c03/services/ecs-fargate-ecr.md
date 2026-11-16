---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - cost
status: to-review
source: notion
notion-module: "11. Containers"
---

# Amazon ECS, Fargate and ECR

## What it is for

Orchestrate containers on AWS (ECS), without servers (Fargate), with a private/public image registry (ECR).

## Key concepts

- Docker: an app and its dependencies in a container; containers share the host kernel (unlike VMs). **ECR**: private or public registry, backed by S3, integrated with ECS/EKS, vulnerability scanning, lifecycle policies, cross-region/account replication. A pull permission error = instance profile or task execution role.
- **Launch types**: *EC2* (you manage the instances running the ECS agent; full control: GPUs, daemons), *Fargate* (nothing to manage; least operational effort), *ECS Managed Instances* (AWS provisions and patches the EC2).
- **IAM roles**: *EC2 instance profile* (EC2 launch type; the agent calls ECS, pushes logs, pulls ECR images, reads Secrets Manager/Parameter Store); **ECS task role** = each task's own permissions (least privilege at container level).
- **Load balancing**: ALB (dynamic port mapping, general case); NLB (high throughput or PrivateLink); CLB not recommended.
- **Volumes**: EFS on EC2 and Fargate (serverless persistent storage); S3 Files on Fargate and Managed Instances.
- **Scaling**: Service Auto Scaling (Application Auto Scaling) on average CPU/memory or ALB RequestCountPerTarget; target tracking, step, scheduled. Scaling tasks does not add EC2 -> **capacity provider** linked to an ASG (preferred over plain CPU scaling).
- **Event-driven patterns**: EventBridge rule on an S3 upload runs a task; EventBridge Scheduler for batches; tasks polling SQS with scaling on queue depth; EventBridge catches stopped/failed tasks -> SNS.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Fargate: you pay for tasks, no fleet to manage. Compute Savings Plans cover Fargate.

## When to use it vs alternatives

- Serverless -> Fargate. Specific hardware or cost at high density -> EC2 launch type. Already on Kubernetes -> [[eks]]. Comparison -> [[ecs-ec2-vs-fargate]]. Job > 15 min -> [[other-services]] (Batch) rather than [[lambda]].

## Exam traps

- Least operational overhead for containers = Fargate.
- Per-task permissions = task role, not instance profile.
- Shared persistent storage for Fargate tasks = EFS.
- Scaling tasks on EC2 needs a capacity provider to add instances.

## See also

[[ecs-ec2-vs-fargate]], [[eks]], [[efs]], [[elb]], [[eventbridge]], [[fsx-and-s3-files]], [[lambda]]
