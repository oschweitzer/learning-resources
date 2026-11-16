---
tags:
  - aws
  - saa-c03
  - resilient
statut: a-revoir
source: notion
module-notion: "12. Serverless"
---

# AWS Step Functions

## À quoi ça sert

Workflows visuels serverless qui orchestrent Lambda et d'autres services (ECS, Batch, DynamoDB, SQS, activités on-premises).

## Concepts clés

- Séquences, branches parallèles, choix, retries, timeouts, gestion d'erreurs, attentes, étapes d'approbation humaine.
- **Standard** : jusqu'à 1 an, exactly-once. **Express** : moins cher, grand volume, courtes durées (jusqu'à 5 min), traitement d'événements.
- Usages : traitement de commandes, pipelines de données, workflows ML, flux d'applications web.

## Limites et quotas

- Standard : jusqu'à 1 an. Express : jusqu'à 5 minutes.

## Tarification (logique)

- Express : moins cher, pour du volume élevé.

## Quand l'utiliser vs alternatives

- Orchestrer plusieurs étapes/services → Step Functions ; découper un job long qui dépasse 15 min de [[lambda]].

## Pièges d'examen

- Standard = exactly-once jusqu'à 1 an ; Express = volume élevé et court.

## Voir aussi

[[lambda]], [[api-gateway]]
