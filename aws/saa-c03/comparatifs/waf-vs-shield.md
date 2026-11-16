---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
status: to-review
source: notion
notion-module: "18. Security and encryption"
---

# WAF vs Shield vs Firewall Manager

Application layer, L3/L4 DDoS and central management.

| Service | Protects against | Remark |
| --- | --- | --- |
| WAF | L7 web exploits (SQL injection, XSS), IPs, geo, rate-based | Not on NLB; web ACL regional except CloudFront |
| Shield Standard | L3/L4 DDoS | Free, automatic |
| Shield Advanced | Sophisticated DDoS on EC2, ELB, CloudFront, GA, Route 53 | 24/7 SRT, scaling cost protection, automatic WAF rules |
| Firewall Manager | Manages WAF, Shield Advanced, SGs, Network Firewall, DNS Firewall across the organisation | Applies to new resources/accounts |

## Decision rule

- SQL injection / XSS -> WAF.
- DDoS support + cost protection -> Shield Advanced.
- Multi-account -> Firewall Manager.

## Exam traps

- SQL injection != Shield.

## See also

[[waf-shield-firewall-manager]]
