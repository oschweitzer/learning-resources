---
tags:
  - aws
  - saa-c03
  - resilient
  - cost
  - secure
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# NAT: instance, zonal NAT gateway and regional NAT gateway

## What it is for

Give private-subnet resources outbound internet access. Route: private subnet 0.0.0.0/0 -> NAT -> IGW.

## Key concepts

- **NAT instance** (legacy, AMI out of standard support since end of 2020, but still on the exam): EC2 in a public subnet with an Elastic IP, **source/destination check disabled**; SG on the instance; HA is up to you; bandwidth depends on the type; can double as a bastion, port forwarding.
- **Zonal NAT gateway**: in a public subnet of **one AZ** with an Elastic IP; HA within the AZ; **one per AZ** for resilience (no cross-AZ failover); 5 Gbps up to 100 Gbps; **NACLs only, no security group**; cannot be used by instances in its own subnet; needs an IGW; supports *private NAT*.
- **Regional NAT gateway** (November 2025): attached to the VPC, **no public subnet needed**; expands/contracts automatically across the AZs where you have workloads; one NAT ID for all AZs, up to 5 per VPC; NACLs; no private NAT.

## Limits and quotas

- Regional NAT: up to 5 per VPC. Zonal NAT: 5 up to 100 Gbps.

## Pricing (logic)

- NAT gateway: per hour + per GB processed. NAT instance: instance hours + traffic.

## When to use it vs alternatives

- For S3/DynamoDB prefer a gateway endpoint (free) -> [[vpc-endpoints]]. Comparison -> [[nat-gateway-vs-instance]].

## Exam traps

- One NAT gateway per AZ for zonal resilience, or a regional NAT gateway.
- NAT gateway = no security group.
- NAT instance: disable the source/destination check.

## See also

[[nat-gateway-vs-instance]], [[vpc]], [[vpc-endpoints]], [[lambda]]
