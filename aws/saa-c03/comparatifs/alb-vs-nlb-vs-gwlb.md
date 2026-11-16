---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "04. High availability and scalability"
---

# ALB vs NLB vs GWLB (vs CLB)

Choisir le type de load balancer.

| Type | Couche | Protocoles | Points clés | Cibles |
| --- | --- | --- | --- | --- |
| ALB | 7 | HTTP, HTTPS, WebSocket, HTTP/2, gRPC | Routage path/host/query/header, redirections, DNS fixe sans IP statique | EC2, ECS, Lambda, IP privées |
| NLB | 4 | TCP, UDP, TLS | Millions req/s, latence ultra basse, IP statique par AZ, IP source préservée | EC2, IP privées, ALB |
| GWLB | 3 | IP, GENEVE (6081) | Appliances tierces transparentes (firewall, IDS/IPS) | EC2, IP privées |
| CLB | 4 et 7 | HTTP, HTTPS, TCP, SSL | Legacy, un seul certificat | EC2 |

## Règle de décision

- Routage par URL/host/header → ALB.
- IP statique, UDP, perf extrême → NLB.
- Flotte d'appliances → GWLB.
- IP statique + HTTP → NLB devant ALB ou Global Accelerator.

## Pièges d'examen

- Plusieurs certificats = ALB/NLB avec SNI (pas CLB).
- Cross-zone : ALB on/gratuit ; NLB/GWLB off/payant si on.

## Voir aussi

[[elb]], [[global-accelerator]]
