---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
status: to-review
source: notion
notion-module: "04. High availability and scalability"
---

# ALB vs NLB vs GWLB (vs CLB)

Choosing the load balancer type.

| Type | Layer | Protocols | Key points | Targets |
| --- | --- | --- | --- | --- |
| ALB | 7 | HTTP, HTTPS, WebSocket, HTTP/2, gRPC | Routing by path/host/query/header, redirects, fixed DNS name without static IP | EC2, ECS, Lambda, private IPs |
| NLB | 4 | TCP, UDP, TLS | Millions of req/s, ultra-low latency, static IP per AZ, source IP preserved | EC2, private IPs, ALB |
| GWLB | 3 | IP, GENEVE (6081) | Transparent third-party appliances (firewall, IDS/IPS) | EC2, private IPs |
| CLB | 4 and 7 | HTTP, HTTPS, TCP, SSL | Legacy, one certificate only | EC2 |

## Decision rule

- Routing by URL/host/header -> ALB.
- Static IP, UDP, extreme performance -> NLB.
- Fleet of appliances -> GWLB.
- Static IP + HTTP -> NLB in front of ALB or Global Accelerator.

## Exam traps

- Several certificates = ALB/NLB with SNI (not CLB).
- Cross-zone: ALB on/free; NLB/GWLB off/charged when on.

## See also

[[elb]], [[global-accelerator]]
