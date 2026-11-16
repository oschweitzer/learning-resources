---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - cost
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# NAT gateway vs NAT instance (vs NAT régionale)

Trois façons d'offrir une sortie internet aux subnets privés.

|  | NAT instance | NAT gateway zonale | NAT gateway régionale |
| --- | --- | --- | --- |
| Statut | Legacy, encore à l'examen | Option managée classique | Lancée en novembre 2025 |
| Placement | EC2 en subnet public + Elastic IP, source/dest check off | Subnet public d'une AZ + Elastic IP | Attachée au VPC, pas de subnet public |
| Disponibilité | À votre charge | HA dans une AZ ; une par AZ, pas de failover cross-AZ | S'étend sur les AZ avec workloads |
| Bande passante | Selon le type d'instance | 5 Gbps, jusqu'à 100 Gbps | Gérée par AWS |
| Sécurité | Security groups | NACL seulement | NACL |
| Autre | Peut servir de bastion, port forwarding | Pas pour les instances de son subnet ; IGW requis ; private NAT possible | Un NAT ID pour toutes les AZ, 5 par VPC ; pas de private NAT |
| Coût | Heures d'instance + trafic | Heure + GB traité | Heure + GB traité |

## Règle de décision

- Managé et résilient sans effort → NAT régionale (ou une zonale par AZ).
- NAT instance seulement pour bastion/port forwarding/contraintes de coût.

## Pièges d'examen

- NAT gateway : pas de SG.
- Une zonale par AZ pour la résilience.

## Voir aussi

[[nat]], [[vpc-endpoints]]
