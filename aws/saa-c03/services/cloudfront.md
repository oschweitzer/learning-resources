---
tags:
  - aws
  - saa-c03
  - performant
  - secure
  - cost
status: to-review
source: notion
notion-module: "08. CloudFront and Global Accelerator"
---

# Amazon CloudFront

## What it is for

CDN that caches content at points of presence: better read performance, DDoS absorption, Shield and WAF integration.

## Key concepts

- **Origins**: *S3 bucket* (secure with **Origin Access Control (OAC)**: private bucket, policy only allows the distribution); *VPC origin* (private ALB, NLB or EC2, no internet exposure); *custom HTTP origin* (S3 website, public ALB, any backend; restrict to CloudFront IP ranges or a secret header + WAF).
- Flow: the edge checks its cache, on a miss fetches from the origin, caches for the TTL, answers. **Origin groups**: failover between primary and secondary.
- **Geo restriction**: allowlist or blocklist of countries (licensing, copyright).
- **Invalidation**: forces a refresh (`/*` or `/images/*`); versioned file names avoid invalidations.
- **CloudFront Functions** and **Lambda@Edge** customise requests/responses. **Signed URLs / signed cookies** for premium content. ACM certificates for CloudFront: **us-east-1**.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- CloudFront egress is cheaper than S3 egress; S3 -> CloudFront is free (see network costs in [[vpc]]).

## When to use it vs alternatives

- Cacheable content worldwide -> CloudFront. Static IPs, UDP, fast failover -> [[global-accelerator]]. Dynamic data read/written in a few regions -> S3 CRR. Comparison -> [[cloudfront-vs-global-accelerator]].

## Exam traps

- Private S3 bucket behind CloudFront: OAC, no public policy.
- Caching = CloudFront.
- CloudFront certificates in us-east-1.
- Blocking an IP behind CloudFront: WAF on CloudFront, not a NACL.

## See also

[[s3]], [[global-accelerator]], [[cloudfront-vs-global-accelerator]], [[waf-shield-firewall-manager]], [[acm]], [[lambda]]
