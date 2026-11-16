---
tags:
  - aws
  - saa-c03
  - secure
  - cost
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# VPC endpoints and PrivateLink

## What it is for

Reach AWS services from private subnets over the AWS network, without IGW or NAT.

## Key concepts

- **Interface endpoint**: an ENI with a private IP protected by an SG; most AWS services + your own and partner services through PrivateLink; paid (hour + GB); reachable from on premises/other VPCs (VPN, Direct Connect, peering).
- **Gateway endpoint**: a target in route tables; **S3 and DynamoDB only**; **free**; not reachable from on premises or other VPCs.
- S3 inside the VPC -> gateway endpoint (free). Interface endpoint preferred when access comes from on premises, another VPC or another region. Lambda in a VPC reaching DynamoDB -> gateway endpoint rather than NAT.
- **Endpoint policies**: restrict what can pass (e.g. one bucket only); bucket policies can require `aws:SourceVpce`.
- **PrivateLink endpoint services**: expose your service (behind an NLB or GWLB) to thousands of consumer VPCs, without peering, CIDR conflicts or internet exposure.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Interface: per hour + per GB; gateway: free.

## When to use it vs alternatives

- Comparison -> [[interface-vs-gateway-endpoint]]. Paid alternative: [[nat]].

## Exam traps

- Gateway endpoints exist only for S3 and DynamoDB and are free.
- Private access to S3 from on premises: interface endpoint.

## See also

[[interface-vs-gateway-endpoint]], [[nat]], [[s3]], [[dynamodb]], [[api-gateway]], [[vpc]]
