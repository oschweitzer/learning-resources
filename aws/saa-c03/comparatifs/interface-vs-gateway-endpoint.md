---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
  - cost
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# Interface endpoint vs Gateway endpoint

Two types of VPC endpoints.

|  | Interface endpoint | Gateway endpoint |
| --- | --- | --- |
| Mechanism | ENI with a private IP, protected by an SG | Target in route tables |
| Services | Most AWS services + your services via PrivateLink | S3 and DynamoDB only |
| Cost | Hour + GB | Free |
| From on premises / other VPCs | Yes (VPN, DX, peering) | No |

## Decision rule

- S3/DynamoDB from the VPC -> gateway (free).
- From on premises, another VPC or another region -> interface.

## Exam traps

- Gateway only for S3 and DynamoDB.

## See also

[[vpc-endpoints]]
