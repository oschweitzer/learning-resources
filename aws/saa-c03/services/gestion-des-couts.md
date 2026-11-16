---
tags:
  - aws
  - saa-c03
  - cost
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# Outils de gestion des coûts

## À quoi ça sert

Visualiser, alerter, optimiser les dépenses AWS (domaine 4).

## Concepts clés

- **Cost Explorer** : visualise coûts et usage, multi-comptes, mensuel/horaire/par ressource ; recommande des Savings Plans ; prévisions.
- **Cost Anomaly Detection** : ML qui apprend vos patterns de dépense et alerte sur pics ou hausses continues, **sans seuils** ; surveille services, comptes, cost allocation tags, cost categories.
- **AWS Budgets** : alertes quand les coûts/usage dépassent ou sont prévus de dépasser un budget ; peut déclencher des actions.
- **Cost and Usage Report** (Data Exports) : données de facturation les plus détaillées, vers S3 pour Athena/Quick. **Cost allocation tags** : découper les coûts par projet/équipe/environnement.
- **Compute Optimizer** : right-sizing pour EC2, ASG, EBS, Lambda, ECS sur Fargate. **Savings Plans / RI** : engagements ([[options-d-achat-ec2]]).
- **Instance Scheduler on AWS** : solution CloudFormation qui stoppe/démarre EC2, ASG et RDS selon des plannings (DynamoDB, tags, Lambda) ; ex. arrêter le dev hors heures de bureau.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Visualiser → Cost Explorer ; alerter sur budget → Budgets ; anomalies sans seuil → Cost Anomaly Detection ; right-sizing → Compute Optimizer.

## Pièges d'examen

- Dépenses inattendues sans seuils = Cost Anomaly Detection.
- Right-sizing = Compute Optimizer.

## Voir aussi

[[options-d-achat-ec2]], [[organizations-et-scp]], [[athena]], [[well-architected-et-trusted-advisor]]
