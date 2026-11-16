---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
  - cost
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# Interface endpoint vs Gateway endpoint

Deux types de VPC endpoints.

|  | Interface endpoint | Gateway endpoint |
| --- | --- | --- |
| Mécanisme | ENI avec IP privée, protégée par SG | Cible dans les route tables |
| Services | La plupart des services AWS + vos services via PrivateLink | S3 et DynamoDB uniquement |
| Coût | Heure + GB | Gratuit |
| Depuis on-premises / autres VPC | Oui (VPN, DX, peering) | Non |

## Règle de décision

- S3/DynamoDB depuis le VPC → gateway (gratuit).
- Depuis on-premises, autre VPC ou autre région → interface.

## Pièges d'examen

- Gateway uniquement S3 et DynamoDB.

## Voir aussi

[[vpc-endpoints]]
