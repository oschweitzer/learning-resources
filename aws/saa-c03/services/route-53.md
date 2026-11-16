---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
status: to-review
source: notion
notion-module: "06. Route 53"
---

# Amazon Route 53

## What it is for

Managed authoritative DNS, domain registrar and health checks. The only AWS service with a 100% availability SLA.

## Key concepts

- Records: name, type, value, routing policy, TTL. **A** (IPv4), **AAAA** (IPv6), **CNAME** (to another hostname, forbidden at the apex), **NS**. Others to recognise: CAA, MX, TXT, SOA, SRV, PTR, DS (the exam mostly tests A, AAAA, CNAME, NS and alias).
- **Hosted zone**: public (internet) or private (one or more VPCs). NS and SOA records are created automatically.
- **TTL**: high = fewer queries and lower cost but slow propagation; low = the opposite. **Lower the TTL before a planned migration.** TTL is mandatory except for alias records.
- **Alias**: points to an AWS resource, allowed at the apex, free queries, native target health evaluation, type A/AAAA. Targets: load balancers, CloudFront, API Gateway, Beanstalk, S3 website, VPC interface endpoints, Global Accelerator, records in the same zone. **You cannot alias an EC2 public DNS name.**
- **Routing policies**: Simple, Weighted (relative weights, 0 = stop), Latency, Failover (health check required on the primary), Geolocation (continent/country/US state; default record needed), Geoproximity (bias -99 to 99), IP-based (CIDR collections), Multi-value (up to 8 healthy values; not an ELB).
- **Health checks**: endpoint (~15 global checkers, every 30 s or 10 s at higher cost, threshold 3, healthy when more than 18% of checkers agree, text match in the first 5,120 bytes), calculated (AND/OR/NOT, up to 256 children), CloudWatch alarm. The endpoint must allow the checker IP ranges. **For a private resource: CloudWatch metric + alarm + health check on the alarm.**
- Domain at a third-party registrar: create the public hosted zone in Route 53, update the NS records at the registrar.
- **Hybrid DNS (Resolver)**: *inbound endpoint* = on-premises resolves AWS names; *outbound endpoint* = the VPC forwards to on-premises resolvers (rules per domain). Needs VPN or Direct Connect; ENIs in 2 AZs.

## Limits and quotas

- Multi-value: 8 healthy values max.
- Calculated health check: 256 children max.
- Geoproximity bias: -99 to 99.

## Pricing (logic)

- Hosted zones are billed monthly; domain registration yearly.
- Alias queries are free (CNAME queries are charged).
- Low TTL = more queries = more cost.

## When to use it vs alternatives

- CNAME vs alias -> [[cname-vs-alias]]. The 8 policies -> [[route53-routing-policies]]. Failover independent of DNS -> [[global-accelerator]].

## Exam traps

- A record at the zone apex pointing to a load balancer needs an alias, never a CNAME.
- Health checks cannot reach private endpoints: use a CloudWatch alarm.
- Geolocation needs a default record.
- Multi-value is not a load balancer.

## See also

[[cname-vs-alias]], [[route53-routing-policies]], [[global-accelerator]], [[cloudfront]], [[acm]], [[vpc]]
