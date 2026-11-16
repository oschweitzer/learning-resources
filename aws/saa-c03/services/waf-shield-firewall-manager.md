---
tags:
  - aws
  - saa-c03
  - secure
  - resilient
status: to-review
source: notion
notion-module: "18. Security and encryption"
---

# AWS WAF, Shield and Firewall Manager

## What it is for

Protect web apps (WAF, layer 7), protect against DDoS (Shield) and manage these rules across the organisation (Firewall Manager).

## Key concepts

- **WAF**: L7 firewall on ALB, API Gateway, CloudFront, AppSync GraphQL, Cognito user pools... **not on NLB** (L4). Web ACL: IP sets (up to 10,000 IPs), headers, body, URI, SQL injection, XSS, size, geo-match; **rate-based rules**; managed rule groups. A web ACL is regional (except CloudFront). Fixed IP + WAF: Global Accelerator in front of an ALB with WAF.
- **Shield Standard**: free and automatic, layers 3 and 4 (SYN/UDP floods, reflection). **Shield Advanced**: (fixed monthly cost per organisation) protects EC2, ELB, CloudFront, Global Accelerator, Route 53 against more sophisticated attacks; 24/7 Shield Response Team; cost protection against scaling bills during attacks; automatic L7 mitigation through WAF rules.
- **Firewall Manager**: security rules across all accounts of an organisation: WAF, Shield Advanced, security groups (EC2, ALB, ENI), Network Firewall, DNS Firewall, third-party firewalls; regional policies applied to new resources/accounts. WAF alone = granular per resource; Firewall Manager = automates at scale; Shield Advanced adds DDoS response and cost protection.
- **DDoS best practices**: edge (CloudFront, Route 53, Global Accelerator); infrastructure (ASG, ELB, large instances); application (CloudFront cache, WAF rate-based/managed rules, geo restriction, Shield Advanced automatic rules); reduce the attack surface (hide backends behind CloudFront/API Gateway/ELB, SGs and NACLs, protect Elastic IPs, API Gateway + WAF).

## Limits and quotas

- WAF IP sets: up to 10,000 IPs.

## Pricing (logic)

- Shield Standard free; Shield Advanced: fixed monthly cost per organisation, with cost protection for scaling during an attack.

## When to use it vs alternatives

- SQL injection / XSS -> WAF (never Shield). 24/7 DDoS support + cost protection -> Shield Advanced. Multi-account -> Firewall Manager. Comparison -> [[waf-vs-shield]].

## Exam traps

- SQL injection protection = WAF, never Shield.
- WAF cannot attach to an NLB.
- Blocking an IP behind CloudFront = WAF.

## See also

[[waf-vs-shield]], [[cloudfront]], [[elb]], [[api-gateway]], [[global-accelerator]], [[vpc-flow-logs-and-network-firewall]], [[security-groups-and-nacl]]
