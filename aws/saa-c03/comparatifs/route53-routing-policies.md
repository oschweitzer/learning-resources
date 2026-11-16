---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
status: to-review
source: notion
notion-module: "06. Route 53"
---

# The 8 Route 53 routing policies

DNS does not route traffic: the policy decides the answer.

| Policy | Behaviour | Health checks | Use cases |
| --- | --- | --- | --- |
| Simple | One or more values, the client picks one at random | No | Single resource |
| Weighted | Split by relative weights (0 = stop) | Yes | Gradual migration, canary |
| Latency | Region with the lowest latency | Yes | Global apps |
| Failover | Primary while healthy, else secondary | Required on the primary | Active-passive DR |
| Geolocation | Continent, country, US state; default record needed | Yes | Localisation, restrictions, compliance |
| Geoproximity | Distance to resources with a bias -99 to 99 | Yes | Shift traffic between regions |
| IP-based | Client CIDR (CIDR collection) | Yes | ISP-specific routing, cost or performance control |
| Multi-value | Up to 8 healthy values | Yes | Client-side spreading; not an ELB |

## Decision rule

- Latency != geolocation: latency = fastest region; geo = where the user is, whatever the latency.

## Exam traps

- Geolocation without a default record = users not covered get no answer.
- Multi-value is not a load balancer.

## See also

[[route-53]]
