---
tags:
  - aws
  - saa-c03
  - resilient
  - cost
  - performant
status: to-review
source: notion
notion-module: "04. High availability and scalability"
---

# EC2 Auto Scaling

## What it is for

Keep the number of instances between a minimum and a maximum around a desired capacity, replace unhealthy instances and react to load.

## Key concepts

- An **ASG** scales out under load, scales in when load drops and replaces unhealthy instances. It is **free** (you pay for the instances).
- It relies on a **launch template** (AMI, type, user data, EBS, SGs, key pair, IAM role, network). Launch configurations are deprecated.
- With ELB health checks enabled, an instance the load balancer marks unhealthy is terminated and replaced. Spread the ASG across several AZs.
- **Policies**: *target tracking* (default, e.g. average CPU at 40%), *step* (adjustment depending on how far the alarm is breached), *simple* (legacy), *scheduled* (known peaks), *predictive* (regular cycles, scales ahead).
- Good metrics: average CPUUtilization, **RequestCountPerTarget**, network in/out, custom metric (e.g. SQS backlog per instance).
- **Cooldown** (300 s by default) stops the group from reacting before metrics stabilise; *instance warm-up* does the same per new instance.
- **Lifecycle hooks**: pause in pending/terminating (install software, copy logs). **Instance refresh**: gradual replacement after a launch template change. **Warm pools**: pre-initialised instances.
- 'HA' single EC2 instance: ASG min=max=desired=1 across 2 AZs, user data attaches the Elastic IP (see [[reference-architectures]]).

## Limits and quotas

- Default cooldown: 300 seconds.

## Pricing (logic)

- The ASG is free. Reserve (RI/Savings Plans) the minimum capacity to save money.

## When to use it vs alternatives

- Target tracking by default. Known load -> scheduled/predictive. For ECS tasks -> see [[ecs-fargate-ecr]] (capacity provider).

## Exam traps

- An ASG is free.
- Target tracking = the default policy.

## See also

[[elb]], [[ami-and-instance-store]], [[cloudwatch]], [[sqs]], [[reference-architectures]]
