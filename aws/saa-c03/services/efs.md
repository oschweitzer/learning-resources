---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
status: to-review
source: notion
notion-module: "03. EC2 storage"
---

# Amazon EFS

## What it is for

Managed NFS file system that hundreds of Linux instances can mount at once, across AZs.

## Key concepts

- NFS v4.1, **POSIX, Linux only** (not Windows). Access controlled by security groups on mount targets. Encryption at rest with KMS.
- Grows and shrinks automatically, no capacity planning. Use cases: CMS, web serving, shared data, WordPress.
- **Performance mode** (set at creation): General Purpose (default, recommended); Max I/O (legacy, higher latency).
- **Throughput modes**: Elastic (automatic, recommended for spiky/unpredictable access), Provisioned (fixed throughput regardless of size), Bursting (tied to stored amount).
- **Classes**: Standard, Infrequent Access (lower storage price, retrieval fee), Archive (read a few times a year). Lifecycle policies move files after N days without access.
- **Regional** (several AZs, production) vs **One Zone** (one AZ, dev/backup, cheaper). One Zone + IA: savings above 90%.
- Mountable on ECS tasks (EC2 and Fargate) and EKS Fargate pods (via CSI) -> serverless persistent storage.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Pay per GB used, more expensive per GB than EBS. IA/Archive classes: cheaper storage, retrieval fees. One Zone is cheaper than Regional.

## When to use it vs alternatives

- Shared Linux multi-AZ -> EFS. Windows/SMB -> [[fsx-and-s3-files]]. One instance's disk -> [[ebs]]. Objects -> [[s3]]. Comparison -> [[ebs-vs-efs-vs-s3]].

## Exam traps

- EFS does not work for Windows instances.
- Max I/O is no longer the default answer: General Purpose with Elastic throughput is.

## See also

[[ebs]], [[fsx-and-s3-files]], [[ebs-vs-efs-vs-s3]], [[ecs-fargate-ecr]], [[eks]]
