---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - resilient
status: to-review
source: notion
notion-module: "08. CloudFront and Global Accelerator"
---

# CloudFront vs Global Accelerator

Both use the global network and edge locations and integrate with Shield.

|  | CloudFront | Global Accelerator |
| --- | --- | --- |
| Content | Cacheable (images, video) and dynamic (API acceleration), served from the edge | No caching; proxies packets to the app in one or more regions |
| Protocols | HTTP and HTTPS | TCP or UDP |
| Fits | Websites, media, APIs | Gaming (UDP), IoT (MQTT), VoIP, HTTP apps needing static IPs or fast regional failover |

## Decision rule

- Caching -> CloudFront.
- Static IPs / UDP / deterministic failover -> Global Accelerator.

## Exam traps

- CloudFront certificates in us-east-1.

## See also

[[cloudfront]], [[global-accelerator]]
