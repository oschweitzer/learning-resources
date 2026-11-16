---
tags:
  - aws
  - saa-c03
  - comparatif
  - cost
  - performant
statut: a-revoir
source: notion
module-notion: "11. Containers"
---

# ECS sur EC2 vs Fargate vs Managed Instances

Choisir le mode de capacité d'ECS.

|  | EC2 launch type | Fargate | ECS Managed Instances |
| --- | --- | --- | --- |
| Infra | Vous gérez les EC2 avec l'agent ECS | Aucune | AWS provisionne et patche l'EC2 |
| Scaling | Tâches + instances (ASG, capacity provider) | Nombre de tâches seulement | Géré par AWS |
| Contrôle | Total (types, GPU, daemons) | Le moins | Plus de choix de types que Fargate |
| Réponse typique | Matériel spécifique ou coût à haute densité | Serverless, moindre effort opérationnel | EC2 managé sans gérer la flotte |

## Règle de décision

- Moins d'opérations → Fargate.
- GPU / hardware spécifique → EC2.
- EC2 managé → Managed Instances.

## Pièges d'examen

- S3 Files : Fargate et Managed Instances, pas le launch type EC2.

## Voir aussi

[[ecs-fargate-ecr]], [[eks]]
