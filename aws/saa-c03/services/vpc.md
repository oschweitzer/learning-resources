---
tags:
  - aws
  - saa-c03
  - secure
  - resilient
  - cost
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# Amazon VPC: basics, CIDR, IPv6 and network costs

## What it is for

Regional private network: public/private subnets, internet access, addressing, data transfer costs.

## Key concepts

- A region groups several AZs. A **VPC is regional** and spans every AZ of its region; a **subnet** is tied to **a single AZ** (trap: the AZ does not contain the VPC).
- **CIDR**: /32 = 1 IP, /24 = 256, /16 = 65,536, 0.0.0.0/0 = every IP; each step down in the mask doubles the range. Private (RFC 1918): 10.0.0.0/8, 172.16.0.0/12 (default VPC uses 172.31.0.0/16), 192.168.0.0/16.
- Every account has a **default VPC** per region (public subnets). For real workloads build your own VPCs. Never overlap CIDRs with other VPCs or on-premises networks you will connect.
- **AWS reserves 5 IPs per subnet** (.0 network, .1 VPC router, .2 DNS, .3 future use, .255 broadcast reserved): a subnet needing 29 IPs needs a /26 (64), not a /27 (32 - 5 = 27).
- **Internet Gateway (IGW)**: horizontally scaled, HA, one per VPC; does nothing without a route. A **public** subnet = route 0.0.0.0/0 -> IGW + public IPs. **Bastion** in a public subnet (SSH from the corporate CIDR; private instances allow the bastion's SG); modern alternative: Session Manager.
- **IPv6**: ~3.4 x 10^38 addresses, all public and routable. IPv4 cannot be disabled in a VPC/subnet; you add IPv6 (dual-stack). If an instance cannot launch in a dual-stack subnet: IPv4 space exhausted (add an IPv4 CIDR). **Egress-only internet gateway**: IPv6 outbound without inbound (NAT equivalent); route ::/0.
- **Network costs (simplified, per GB)**: internet ingress free; same AZ over private IP free; inter-AZ charged each way over private IPs (about double over public/Elastic IPs) -> use private IPs, same AZ when HA is not required; inter-region charged; internet egress charged after a monthly free allowance shared across the account; keep traffic inside AWS, run queries where the data is; Direct Connect: cheaper egress. S3: ingress free, internet egress charged, Transfer Acceleration extra, **S3 -> CloudFront free** and CloudFront egress cheaper than S3's; CRR charged. **NAT gateway**: per-GB processing on top of transfer; **S3/DynamoDB gateway endpoint free** -> classic optimisation.
- **VPC Block Public Access**: can block internet traffic for whole VPCs or subnets at account level.

## Limits and quotas

- 5 VPCs per region (soft); up to 5 IPv4 CIDRs per VPC, each between /28 (16 IPs) and /16 (65,536 IPs).
- 5 reserved IPs per subnet.

## Pricing (logic)

- Internet ingress free; inter-AZ and inter-region traffic charged; NAT billed per hour and per GB; gateway endpoints free.

## When to use it vs alternatives

- Outbound access -> [[nat]]; private access to AWS services -> [[vpc-endpoints]]; firewalls -> [[security-groups-and-nacl]]; interconnection -> [[vpc-connectivity]].

## Exam traps

- The VPC is regional, subnets are tied to an AZ.
- A /27 only offers 27 usable IPs.
- Free gateway endpoints = classic cost optimisation.

## See also

[[nat]], [[vpc-endpoints]], [[security-groups-and-nacl]], [[vpc-connectivity]], [[vpc-flow-logs-and-network-firewall]]
