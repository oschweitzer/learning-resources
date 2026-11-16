---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# Lambda vs AWS Batch

Quand un job ne rentre plus dans Lambda.

| Lambda | AWS Batch |
| --- | --- |
| Limite de 15 minutes | Pas de limite de temps |
| Runtimes limités | Tout runtime packagé en image Docker |
| Disque temporaire limité (/tmp jusqu'à 10 Go) | EBS ou instance store pour le disque |
| Serverless | S'appuie sur EC2 (géré par AWS) ou Fargate |

## Règle de décision

- Court, événementiel → Lambda.
- Long, gros disque, image custom → Batch (ou Fargate).

## Pièges d'examen

- Job > 15 min ne tourne pas sur Lambda.

## Voir aussi

[[lambda]], [[services-divers]], [[ecs-fargate-ecr]]
