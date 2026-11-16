---
tags:
  - aws
  - saa-c03
  - secure
  - cost
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# VPC endpoints et PrivateLink

## À quoi ça sert

Atteindre des services AWS depuis des subnets privés par le réseau AWS, sans IGW ni NAT.

## Concepts clés

- **Interface endpoint** : une ENI avec IP privée protégée par un SG ; la plupart des services AWS + vos propres services et partenaires via PrivateLink ; payant (heure + GB) ; joignable depuis on-premises/autres VPC (VPN, Direct Connect, peering).
- **Gateway endpoint** : cible dans les route tables ; **S3 et DynamoDB uniquement** ; **gratuit** ; pas joignable depuis on-premises ni d'autres VPC.
- S3 dans le VPC → gateway endpoint (gratuit). Interface endpoint préférée si l'accès vient d'on-premises, d'un autre VPC ou d'une autre région. Lambda en VPC vers DynamoDB → gateway endpoint plutôt que NAT.
- **Endpoint policies** : restreignent ce qui passe (ex. un seul bucket) ; bucket policies peuvent exiger `aws:SourceVpce`.
- **PrivateLink endpoint services** : exposer votre service (derrière NLB ou GWLB) à des milliers de VPC consommateurs, sans peering, sans conflit de CIDR, sans exposition internet.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Interface : à l'heure + au GB ; gateway : gratuit.

## Quand l'utiliser vs alternatives

- Comparatif → [[interface-vs-gateway-endpoint]]. Alternative payante : [[nat]].

## Pièges d'examen

- Les gateway endpoints n'existent que pour S3 et DynamoDB et sont gratuits.
- Depuis on-premises vers S3 en privé : interface endpoint.

## Voir aussi

[[interface-vs-gateway-endpoint]], [[nat]], [[s3]], [[dynamodb]], [[api-gateway]], [[vpc]]
