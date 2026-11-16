---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "11. Containers"
---

# Amazon EKS

## À quoi ça sert

Plan de contrôle Kubernetes managé ; alternative à ECS avec une API open source et cloud-agnostique (multi-cloud, migration Kubernetes).

## Concepts clés

- Exécute les workloads sur nœuds EC2 ou Fargate ; expose les services via ELB.
- **Types de nœuds** : *managed node groups* (EKS crée/gère les EC2 dans un ASG, On-Demand ou Spot), *self-managed* (vous créez, enregistrez, gérez l'ASG ; AMI optimisée EKS), *Fargate* (aucun nœud visible), *EKS Auto Mode* (AWS gère nœuds, scaling, add-ons : moins d'opérations avec la flexibilité EC2).
- **Stockage** : StorageClass avec driver **CSI** ; backends EBS, EFS (le seul compatible Fargate), FSx for Lustre, FSx for NetApp ONTAP.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Déjà Kubernetes ou multi-cloud → EKS ; sinon [[ecs-fargate-ecr]] est plus simple.

## Pièges d'examen

- Déjà Kubernetes / multi-cloud = EKS.
- Seul EFS marche avec les pods Fargate.

## Voir aussi

[[ecs-fargate-ecr]], [[efs]]
