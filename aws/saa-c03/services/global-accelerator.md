---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
status: to-review
source: notion
notion-module: "08. CloudFront and Global Accelerator"
---

# AWS Global Accelerator

## What it is for

Bring users onto the AWS network at the nearest edge location (fewer public internet hops).

## Key concepts

- Gives you **two static anycast IPs** (anycast = many servers share the same IP, the client reaches the nearest).
- Endpoints: Elastic IPs, EC2, ALBs, NLBs, public or private, in one or more regions.
- Health checks drive failover to a healthy region **in under a minute**; the IPs never change, so no DNS cache problem.
- Security: only two IPs to allowlist; Shield protection.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Any TCP/UDP, gaming (UDP), IoT (MQTT), voice over IP, HTTP apps needing static IPs or deterministic regional failover -> Global Accelerator. Caching -> [[cloudfront]].

## Exam traps

- Static IPs or UDP = Global Accelerator.
- It caches nothing.

## See also

[[cloudfront]], [[cloudfront-vs-global-accelerator]], [[elb]], [[route-53]]
