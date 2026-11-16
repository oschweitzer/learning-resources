---
tags:
  - aws
  - saa-c03
  - performant
  - cost
  - resilient
status: to-review
source: notion
notion-module: "02. EC2 fundamentals"
---

# Amazon EC2: instances, purchasing, placement

## What it is for

Virtual servers: pick the instance family, the purchasing option, the placement and the networking.

## Key concepts

- Instance configuration: OS, CPU/RAM through the instance type, storage (EBS, EFS or instance store), security groups, IAM role, key pair, **user data**.
- **User data**: a bootstrap script that runs **once**, as root, at the very first start.
- **IMDS** (169.254.169.254) exposes instance information and role credentials. Enforce **IMDSv2** (session tokens, blocks a class of SSRF attacks).
- The name m7g.2xlarge reads as class m, generation 7, attribute g (Graviton, ARM), size 2xlarge. Other attributes: a (AMD), i (Intel), d (local NVMe), n (enhanced networking).
- Families: general purpose (t, m; t = burstable with CPU credits), compute (c), memory (r, x, z, u), storage (i, d, h), accelerated (p, g, inf, trn). Graviton usually gives the best price/performance if the workload runs on ARM.
- **Security group**: stateful firewall attached to an ENI, *allow* rules only; inbound denied and outbound allowed by default. Rules reference an IP range or another SG. A timeout points to an SG/NACL problem; 'connection refused' points to the application.
- Ports to know: 22 SSH/SFTP, 21 FTP, 80/443 HTTP(S), 3389 RDP, 3306 MySQL/MariaDB/Aurora MySQL, 5432 PostgreSQL/Aurora PostgreSQL, 1433 SQL Server, 1521 Oracle.
- Connecting: SSH (port 22), EC2 Instance Connect (port 22 still open), **Session Manager** (no port, no key, no bastion, sessions logged -> the preferred answer).
- **Spot**: two-minute warning before reclaim (stop or terminate) -> design jobs to checkpoint. Persistent Spot request: cancel the request **first**, then terminate the instances. Spot Fleet / EC2 Fleet: `price-capacity-optimized` is the recommended strategy.
- **IPs**: private IP; public IPv4 changes on every stop/start. **Elastic IP** = static public IPv4, remappable (masks a failure). Every public IPv4 is billed, attached or not. Prefer DNS or a load balancer.
- **ENI**: virtual network card bound to **one AZ**, primary private IPv4 + secondary IPs, one Elastic IP per private IP, SGs, MAC. Movable between instances for failover.
- **Hibernate**: writes RAM to the root EBS volume, so restart resumes processes and warm caches. Needs an encrypted root EBS volume large enough for RAM, RAM < 150 GB, no bare metal. Max 60 days hibernated.

## Limits and quotas

- Elastic IP: 5 per region by default.
- Spread placement group: 7 instances per AZ per group.
- Hibernation: 60 days max, RAM < 150 GB.

## Pricing (logic)

- On-Demand: per-second billing for Linux and Windows after the first minute, no commitment.
- The longer the commitment and the more paid upfront (all upfront, 3 years), the bigger the discount.
- Capacity Reservation: billed whether used or not, no discount (combine with Savings Plans or regional RIs).
- Dedicated Host: the most expensive option.

## When to use it vs alternatives

- Purchasing options -> [[ec2-purchasing-options]].
- Serverless containers -> [[ecs-fargate-ecr]]; event-driven code -> [[lambda]].

## Exam traps

- Spot is never right for a critical database or an uninterruptible job.
- Capacity Reservations give capacity, not a discount.
- Dedicated Host = licences per socket/core.
- User data runs only at first boot.

## See also

[[ec2-purchasing-options]], [[ebs]], [[ami-and-instance-store]], [[security-groups-and-nacl]], [[auto-scaling]], [[systems-manager]]
