---
tags:
  - aws
  - saa-c03
  - resilient
  - cost
  - performant
statut: a-revoir
source: notion
module-notion: "04. High availability and scalability"
---

# EC2 Auto Scaling

## À quoi ça sert

Garder le nombre d'instances entre un min et un max, autour d'une capacité désirée, remplacer les instances malsaines et réagir à la charge.

## Concepts clés

- Un **ASG** scale out sous charge, scale in quand elle baisse et remplace les instances unhealthy. **Gratuit** (on paie les instances).
- Repose sur un **launch template** (AMI, type, user data, EBS, SGs, key pair, IAM role, réseau). Les launch configurations sont dépréciées.
- Avec les health checks ELB activés, une instance jugée unhealthy par le LB est terminée et remplacée. Répartir l'ASG sur plusieurs AZ.
- **Politiques** : *target tracking* (défaut, ex. CPU moyen à 40 %), *step* (ajustement selon l'ampleur du breach d'alarme), *simple* (legacy), *scheduled* (pics connus), *predictive* (cycles réguliers, scale en avance).
- Bonnes métriques : CPUUtilization moyen, **RequestCountPerTarget**, réseau in/out, métrique custom (ex. backlog SQS par instance).
- **Cooldown** (300 s par défaut) évite de réagir avant stabilisation ; *instance warm-up* idem par instance.
- **Lifecycle hooks** : pause en pending/terminating (installer un logiciel, copier des logs). **Instance refresh** : remplacement progressif après changement de launch template. **Warm pools** : instances pré-initialisées.
- Instance EC2 unique « HA » : ASG min=max=desired=1 sur 2 AZ, user data attache l'Elastic IP (voir [[architectures-de-reference]]).

## Limites et quotas

- Cooldown par défaut : 300 secondes.

## Tarification (logique)

- L'ASG est gratuit. Réserver (RI/Savings Plans) la capacité minimale pour économiser.

## Quand l'utiliser vs alternatives

- Target tracking par défaut. Charge connue → scheduled/predictive. Pour des tâches ECS → voir [[ecs-fargate-ecr]] (capacity provider).

## Pièges d'examen

- Un ASG est gratuit.
- Target tracking = politique par défaut.

## Voir aussi

[[elb]], [[ami-et-instance-store]], [[cloudwatch]], [[sqs]], [[architectures-de-reference]]
