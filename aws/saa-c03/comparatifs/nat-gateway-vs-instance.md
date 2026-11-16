---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - cost
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# NAT gateway vs NAT instance (vs regional NAT)

Three ways to give private subnets outbound internet access.

|  | NAT instance | Zonal NAT gateway | Regional NAT gateway |
| --- | --- | --- | --- |
| Status | Legacy, still on the exam | Classic managed option | Launched November 2025 |
| Placement | EC2 in a public subnet + Elastic IP, source/dest check off | Public subnet of one AZ + Elastic IP | Attached to the VPC, no public subnet |
| Availability | Up to you | HA within one AZ; one per AZ, no cross-AZ failover | Expands across the AZs with workloads |
| Bandwidth | Depends on instance type | 5 Gbps, up to 100 Gbps | Managed by AWS |
| Security | Security groups | NACLs only | NACLs |
| Other | Can act as a bastion, port forwarding | Not for instances in its own subnet; IGW required; private NAT possible | One NAT ID for all AZs, 5 per VPC; no private NAT |
| Cost | Instance hours + traffic | Hour + per GB processed | Hour + per GB processed |

## Decision rule

- Managed and resilient without effort -> regional NAT (or one zonal per AZ).
- NAT instance only for bastion/port forwarding/cost constraints.

## Exam traps

- NAT gateway: no SG.
- One zonal NAT per AZ for resilience.

## See also

[[nat]], [[vpc-endpoints]]
