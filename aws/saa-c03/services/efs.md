---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "03. EC2 storage"
---

# Amazon EFS

## À quoi ça sert

File system NFS managé, partagé entre des centaines d'instances Linux sur plusieurs AZ.

## Concepts clés

- NFS v4.1, **POSIX, Linux uniquement** (pas Windows). Accès par security groups sur les mount targets. Chiffrement au repos via KMS.
- Croît et décroît automatiquement, pas de planification de capacité. Usages : CMS, web serving, données partagées, WordPress.
- **Performance mode** (à la création) : General Purpose (défaut, recommandé) ; Max I/O (legacy, latence plus haute).
- **Throughput modes** : Elastic (auto, recommandé pour trafic spiky/imprévisible), Provisioned (débit fixe indépendant de la taille), Bursting (lié à la quantité stockée).
- **Classes** : Standard, Infrequent Access (stockage moins cher + frais de lecture), Archive (quelques accès par an). Lifecycle policies : déplacement après N jours sans accès.
- **Regional** (plusieurs AZ, production) vs **One Zone** (une AZ, dev/backup, moins cher). One Zone + IA : plus de 90 % d'économie.
- Montable sur tâches ECS (EC2 et Fargate) et pods EKS Fargate (via CSI) → stockage persistant serverless.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Paiement au GB utilisé, plus cher par GB qu'EBS. Classes IA/Archive : stockage moins cher, frais de lecture. One Zone moins cher que Regional.

## Quand l'utiliser vs alternatives

- Partagé Linux multi-AZ → EFS. Windows/SMB → [[fsx-et-s3-files]]. Disque d'une instance → [[ebs]]. Objets → [[s3]]. Comparaison → [[ebs-vs-efs-vs-s3]].

## Pièges d'examen

- EFS ne fonctionne pas pour des instances Windows.
- Max I/O n'est plus la réponse par défaut : General Purpose + Elastic throughput.

## Voir aussi

[[ebs]], [[fsx-et-s3-files]], [[ebs-vs-efs-vs-s3]], [[ecs-fargate-ecr]], [[eks]]
