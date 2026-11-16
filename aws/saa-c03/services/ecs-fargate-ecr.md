---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "11. Containers"
---

# Amazon ECS, Fargate et ECR

## À quoi ça sert

Orchestrer des conteneurs sur AWS (ECS), sans serveur (Fargate), avec un registre d'images privé/public (ECR).

## Concepts clés

- Docker : app + dépendances dans un conteneur ; les conteneurs partagent le noyau de l'hôte (≠ VM). **ECR** : registre privé ou public, adossé à S3, intégré à ECS/EKS, scan de vulnérabilités, lifecycle policies, réplication cross-région/compte. Erreur de pull = instance profile ou task execution role.
- **Launch types** : *EC2* (vous gérez les instances avec l'agent ECS ; contrôle total : GPU, daemons), *Fargate* (rien à gérer ; moindre effort opérationnel), *ECS Managed Instances* (AWS provisionne et patche l'EC2).
- **Rôles IAM** : *EC2 instance profile* (launch type EC2 ; l'agent appelle ECS, pousse les logs, tire les images ECR, lit Secrets Manager/Parameter Store) ; **ECS task role** = permissions propres à chaque tâche (moindre privilège au niveau conteneur).
- **Load balancing** : ALB (dynamic port mapping, cas général) ; NLB (haut débit ou PrivateLink) ; CLB déconseillé.
- **Volumes** : EFS sur EC2 et Fargate (persistant serverless) ; S3 Files sur Fargate et Managed Instances.
- **Scaling** : Service Auto Scaling (Application Auto Scaling) sur CPU/mémoire moyens ou RequestCountPerTarget ; target tracking, step, scheduled. Scaler les tâches n'ajoute pas d'EC2 → **capacity provider** lié à un ASG (préféré au scaling CPU simple).
- **Patterns événementiels** : EventBridge sur upload S3 lance une tâche ; EventBridge Scheduler pour des batchs ; tâches qui lisent SQS avec scaling sur la profondeur ; EventBridge détecte les tâches stoppées/échouées → SNS.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Fargate : on paie les tâches, pas de flotte à gérer. Les Compute Savings Plans couvrent Fargate.

## Quand l'utiliser vs alternatives

- Sans serveur → Fargate. Matériel spécifique ou coût à haute densité → EC2 launch type. Déjà Kubernetes → [[eks]]. Comparatif → [[ecs-ec2-vs-fargate]]. Job > 15 min → [[services-divers]] (Batch) plutôt que [[lambda]].

## Pièges d'examen

- Moins d'effort opérationnel = Fargate.
- Permissions par tâche = task role, pas instance profile.
- Stockage partagé persistant pour Fargate = EFS.
- Tâches sur EC2 : capacity provider pour ajouter des instances.

## Voir aussi

[[ecs-ec2-vs-fargate]], [[eks]], [[efs]], [[elb]], [[eventbridge]], [[fsx-et-s3-files]], [[lambda]]
