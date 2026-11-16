---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "08. CloudFront and Global Accelerator"
---

# AWS Global Accelerator

## À quoi ça sert

Amener les utilisateurs sur le réseau AWS au edge le plus proche (moins de sauts internet publics).

## Concepts clés

- Donne **2 IP anycast statiques** (anycast = plusieurs serveurs partagent la même IP, le client atteint le plus proche).
- Endpoints : Elastic IP, EC2, ALB, NLB, publics ou privés, dans une ou plusieurs régions.
- Health checks → failover vers une région saine **en moins d'une minute** ; les IP ne changent jamais, donc pas de problème de cache DNS.
- Sécurité : seulement 2 IP à allowlister ; protection Shield.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- TCP/UDP quelconque, jeux (UDP), IoT (MQTT), VoIP, appli HTTP avec IP statiques ou failover régional déterministe → Global Accelerator. Cache → [[cloudfront]].

## Pièges d'examen

- IP statiques ou UDP = Global Accelerator.
- Il ne met rien en cache.

## Voir aussi

[[cloudfront]], [[cloudfront-vs-global-accelerator]], [[elb]], [[route-53]]
