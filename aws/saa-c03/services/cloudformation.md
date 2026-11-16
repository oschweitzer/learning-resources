---
tags:
  - aws
  - saa-c03
  - cost
  - resilient
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# AWS CloudFormation

## À quoi ça sert

Infrastructure as code déclarative : décrire les ressources, CloudFormation les crée dans le bon ordre.

## Concepts clés

- Bénéfices : contrôle et revue de code des changements, ressources taguées par stack (suivi des coûts), estimation de coût depuis les templates, suppression/recréation planifiée (dev la nuit), diagrammes auto, réutilisation de templates.
- **Infrastructure Composer** visualise templates et relations. **Service role** : CloudFormation crée/modifie/supprime en votre nom ; les users n'ont besoin que d'`iam:PassRole` (moindre privilège). **StackSets** : stacks sur plusieurs comptes et régions.
- CDK hors scope de l'examen, mais synthétise des templates CloudFormation.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Estimation de coût possible depuis les templates ; tags par stack pour l'allocation des coûts.

## Quand l'utiliser vs alternatives

- IaC pour recréer vite un environnement (DR) ; Beanstalk plus guidé ([[elastic-beanstalk]]).

## Pièges d'examen

- Déployer sans permissions directes sur les ressources = service role + iam:PassRole.

## Voir aussi

[[control-tower]], [[dr-et-multi-region]], [[elastic-beanstalk]]
