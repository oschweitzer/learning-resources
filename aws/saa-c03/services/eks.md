---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
status: to-review
source: notion
notion-module: "11. Containers"
---

# Amazon EKS

## What it is for

Managed Kubernetes control plane; an alternative to ECS with an open-source, cloud-agnostic API (multi-cloud, Kubernetes migrations).

## Key concepts

- Runs workloads on EC2 nodes or Fargate; exposes services through ELB.
- **Node types**: *managed node groups* (EKS creates and manages EC2 nodes in an ASG, On-Demand or Spot), *self-managed* (you create nodes, register them, run the ASG; EKS optimised AMI), *Fargate* (no nodes to see), *EKS Auto Mode* (AWS manages nodes, scaling and add-ons: least operations with EC2 flexibility).
- **Storage**: StorageClass using a **CSI** driver; backends EBS, EFS (the only one that works with Fargate), FSx for Lustre, FSx for NetApp ONTAP.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Already Kubernetes or multi-cloud -> EKS; otherwise [[ecs-fargate-ecr]] is simpler.

## Exam traps

- Already Kubernetes / multi-cloud = EKS.
- Only EFS works with Fargate pods.

## See also

[[ecs-fargate-ecr]], [[efs]]
