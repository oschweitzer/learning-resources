---
tags:
  - aws
  - saa-c03
  - resilient
  - cost
  - secure
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# NAT : instance, NAT gateway zonale et régionale

## À quoi ça sert

Donner un accès internet sortant à des ressources de subnets privés. Route : 0.0.0.0/0 du subnet privé → NAT → IGW.

## Concepts clés

- **NAT instance** (legacy, AMI hors support standard fin 2020, mais à l'examen) : EC2 en subnet public avec Elastic IP, **source/destination check désactivé** ; SG sur l'instance ; HA à votre charge ; bande passante selon le type ; peut servir de bastion, port forwarding.
- **NAT gateway zonale** : dans un subnet public d'**une AZ** avec Elastic IP ; HA dans l'AZ ; **une par AZ** pour la résilience (pas de failover cross-AZ) ; 5 Gbps jusqu'à 100 Gbps ; **NACL seulement, pas de SG** ; inutilisable par les instances de son propre subnet ; nécessite un IGW ; supporte *private NAT*.
- **NAT gateway régionale** (novembre 2025) : attachée au VPC, **sans subnet public** ; s'étend/se contracte sur les AZ où vous avez des workloads ; un seul NAT ID pour toutes les AZ, jusqu'à 5 par VPC ; NACLs ; pas de private NAT.

## Limites et quotas

- NAT régionale : jusqu'à 5 par VPC. NAT zonale : 5 à 100 Gbps.

## Tarification (logique)

- NAT gateway : à l'heure + au GB traité. NAT instance : heures d'instance + trafic.

## Quand l'utiliser vs alternatives

- Pour S3/DynamoDB, préférer un gateway endpoint (gratuit) → [[vpc-endpoints]]. Comparatif → [[nat-gateway-vs-instance]].

## Pièges d'examen

- Une NAT gateway par AZ pour la résilience zonale, ou une NAT régionale.
- NAT gateway = pas de security group.
- NAT instance : désactiver source/destination check.

## Voir aussi

[[nat-gateway-vs-instance]], [[vpc]], [[vpc-endpoints]], [[lambda]]
