---
tags:
  - aws
  - saa-c03
  - resilient
statut: a-revoir
source: notion
module-notion: "16. Monitoring and audit"
---

# Amazon EventBridge

## À quoi ça sert

Réagir aux événements (event patterns) et planifier des actions (schedules). Ex-CloudWatch Events.

## Concepts clés

- Sources : presque tous les services AWS (changements d'état EC2, uploads S3, findings GuardDuty, échecs CodeBuild, Trusted Advisor), partenaires SaaS, vos applis. Cibles : Lambda, SQS, SNS, Kinesis, Step Functions, tâches ECS, Batch, actions EC2, API destinations…
- **Event buses** : default (événements AWS), partner (SaaS), custom ; partageables cross-account par resource-based policies (agréger les événements d'une organisation).
- **Archive et replay** (tout ou filtré, durée choisie). **Schema registry** : infère les schémas, génère des bindings de code, versioning.
- **Sécurité** : cibles Lambda/SNS/SQS/S3-related → resource-based policy sur la cible ; cibles ECS/Kinesis/action ASG → IAM role.
- **EventBridge Scheduler** et **Pipes** : planifications ponctuelles/récurrentes et intégrations point à point avec filtrage/enrichissement.
- Avec CloudTrail : intercepte n'importe quel appel d'API (ex. suppression d'une table DynamoDB) et alerte via SNS. Exemple : connexion du root user → SNS email.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Réagir à un événement ou planifier → EventBridge ; notifier plusieurs abonnés → [[sns]] ; S3 vers EventBridge pour filtrage avancé et replay.

## Pièges d'examen

- Notifier la sécurité à la connexion du root = règle EventBridge + SNS.

## Voir aussi

[[cloudtrail]], [[sns]], [[lambda]], [[ecs-fargate-ecr]], [[s3]]
