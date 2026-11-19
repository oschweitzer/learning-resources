---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - cost
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# Reference architectures and patterns

## What it is for

Putting services together: stateless/stateful web, WordPress, event-driven, caching, IP blocking, HPC, highly available single EC2.

## Key concepts

- **Stateless** (WhatIsTheTime.com): 1 EC2 + Elastic IP (SPOF) -> scale vertically then horizontally with Route 53 A records (clients cache the TTL) -> ELB + ASG across AZs (alias, SGs from the LB) -> reserve the minimum capacity.
- **Stateful** (MyClothes.com): sticky sessions (break when the instance dies) -> cart in cookies (stateless app but heavier requests, cookies must be validated) -> session ID in a cookie + session in ElastiCache/DynamoDB -> user data in RDS (read replicas or ElastiCache lazy loading) -> Multi-AZ RDS/ElastiCache, ELB/ASG across AZs, chained SGs.
- **WordPress**: Aurora MySQL Multi-AZ + read replicas; images on EBS are fine for one instance but not several -> shared **EFS** across AZs.
- **Launching quickly**: EC2 golden AMI + user data (or both: Beanstalk); RDS from snapshot; EBS from snapshot.
- **Event-driven**: SQS -> Lambda (DLQ on the SQS queue, watch retry loops); SQS FIFO -> Lambda (a failing message blocks its group); SNS -> Lambda (asynchronous, retries inside Lambda, DLQ on the function); SNS -> SQS fan-out; S3 events (SNS/SQS/Lambda) or S3 -> EventBridge; EventBridge + CloudTrail to intercept an API call; API Gateway straight to Kinesis -> Firehose -> S3 without Lambda.
- **Caching**: at the edge (CloudFront; TTL, invalidation); at the API (API Gateway regional caching); at application/database (ElastiCache, DAX); static content only via CloudFront, dynamic with short TTL. Every layer trades freshness against latency, load and cost.
- **Blocking an IP**: public EC2 -> NACL deny (SGs cannot deny); ALB in front of EC2 -> NACL on the ALB's subnet (instances only see the ALB); NLB in front of EC2 -> NACL (client IPs preserved); ALB with WAF -> WAF IP set rule; CloudFront in front of ALB -> WAF on CloudFront (NACLs do not help, traffic comes from edge IPs).
- **HPC**: transfer (Direct Connect, DataSync, Snowball existing customers); compute (CPU/GPU, Spot Fleets + ASG, **cluster placement groups**); **enhanced networking (SR-IOV)**: ENA; **EFA** = improved ENA for HPC on Linux, bypasses the OS, tightly coupled MPI; storage (io2 Block Express 256,000 IOPS, instance store, S3, EFS, FSx for Lustre); orchestration (Batch multi-node, ParallelCluster open source with EFA from a config file).
- **Highly available single EC2**: option 1 CloudWatch alarm/EventBridge -> Lambda starts a standby instance and moves the Elastic IP; option 2 ASG min=max=desired=1 across 2 AZs, user data attaches the Elastic IP (instance role); for EBS: a lifecycle hook snapshots on termination and a launch hook recreates the volume from the snapshot (tagged).

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- See the notes of each service mentioned.

## Exam traps

- Blocking an IP behind CloudFront = WAF, not NACL.
- Tightly coupled HPC = cluster placement group + EFA.
- The DLQ for SNS -> Lambda goes on the Lambda.

## See also

[[elb]], [[auto-scaling]], [[elasticache]], [[efs]], [[sqs]], [[sns]], [[eventbridge]], [[waf-shield-firewall-manager]], [[security-groups-and-nacl]], [[ec2]], [[lambda]]
