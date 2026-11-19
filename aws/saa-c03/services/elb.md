---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
status: to-review
source: notion
notion-module: "04. High availability and scalability"
---

# Elastic Load Balancing (ALB, NLB, GWLB, CLB)

## What it is for

Spread traffic over several targets with one DNS name, health checks, TLS, stickiness and multi-AZ. ELB is managed (upgrades, scaling, availability).

## Key concepts

- **ALB** (L7): HTTP/HTTPS/WebSocket/HTTP2/gRPC, routing by path, host, query string, header; redirects; fixed DNS name, **no static IP**. Targets: EC2, ECS tasks (dynamic port mapping), Lambda, private IPs (including on premises).
- **NLB** (L4): TCP/UDP/TLS, millions of requests per second, ultra-low latency, **one static IP per AZ** (Elastic IP possible), keeps the client source IP, supports security groups. Targets: EC2, private IPs, an ALB.
- **GWLB** (L3): IP packets, GENEVE port 6081, sends traffic through a fleet of third-party appliances (firewalls, IDS/IPS, DPI).
- **CLB**: legacy (L4 and L7), one certificate only.
- Behind an ALB the client IP is in `X-Forwarded-For` (+ `X-Forwarded-Port`, `X-Forwarded-Proto`).
- **Health checks** per target group (port + path, e.g. /health); non-200 = unhealthy. An NLB supports TCP, HTTP, HTTPS.
- Security pattern: the LB's SG allows 80/443 from anywhere; the instances' SG only allows the LB's SG.
- **Sticky sessions** (CLB, ALB, NLB): application cookies (custom or AWSALBAPP) or duration cookies (AWSALB, AWSELB for CLB). Reserved names: AWSALB, AWSALBAPP, AWSALBTG. Can unbalance load; better: stateless app with sessions in ElastiCache/DynamoDB.
- **Cross-zone**: ALB on by default (can be off per target group, free); NLB and GWLB off by default (inter-AZ charges if on); CLB off by default, no charge.
- **TLS**: X.509 certificate, usually from ACM. **SNI**: several certificates on one listener (ALB, NLB, CloudFront). CLB does not support it.
- **Deregistration delay** (connection draining on CLB): lets in-flight requests finish. Default 300 s, range 0 to 3,600 s, 0 disables; lower it (~30 s) for short requests.

## Limits and quotas

- Deregistration delay: 0 to 3,600 s (default 300).
- NLB: one static IP per AZ.

## Pricing (logic)

- Cross-zone: free on ALB; NLB/GWLB charge inter-AZ data when it is turned on.

## When to use it vs alternatives

- Detailed comparison -> [[alb-vs-nlb-vs-gwlb]].
- Static IP in front of an HTTP app: an NLB in front of an ALB, or [[global-accelerator]].

## Exam traps

- Routing by path/host = ALB.
- Static IP, UDP or extreme performance = NLB.
- Third-party firewall appliances = GWLB.
- Several TLS certificates on one LB = ALB/NLB with SNI.

## See also

[[alb-vs-nlb-vs-gwlb]], [[auto-scaling]], [[acm]], [[global-accelerator]], [[route-53]]
