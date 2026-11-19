---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
status: to-review
source: notion
notion-module: "06. Route 53"
---

# CNAME vs Alias (Route 53)

Two ways to point one name at another.

|  | CNAME | Alias |
| --- | --- | --- |
| Points to | Any hostname | An AWS resource |
| Zone apex | Forbidden | Allowed |
| Cost | Queries charged | Queries free |
| Health check | Possible on the record | Native target health evaluation |
| Record type | CNAME | A or AAAA |

## Decision rule

- Apex to LB/CloudFront -> alias.
- Target outside AWS (SaaS) -> CNAME (on a subdomain).
- A CNAME is alone on its name (no TXT/MX alongside).

## Exam traps

- example.com CNAME other.com is forbidden (the apex holds SOA and NS).
- No alias to an EC2 public DNS name.

## See also

[[route-53]]
