---
tags:
  - aws
  - saa-c03
  - resilient
  - secure
statut: a-revoir
source: notion
module-notion: "20. Disaster recovery, migration and multi-region"
---

# AWS Backup

## À quoi ça sert

Sauvegardes centralisées et managées, sans scripts, sur de nombreux services.

## Concepts clés

- Supporte EC2, EBS, S3, RDS (tous moteurs), Aurora, DynamoDB, DocumentDB, Neptune, EFS, FSx (Lustre, Windows, ONTAP, OpenZFS), volumes Storage Gateway, etc.
- Copies cross-région et cross-compte ; PITR pour les services qui le supportent ; backups à la demande ou planifiés ; policies par tags.
- **Backup plans** : fréquence (12 h, quotidien, hebdo, mensuel, cron), fenêtre, transition vers froid, rétention.
- **Vault Lock** : WORM sur un vault ; personne, pas même root, ne peut supprimer ni raccourcir la rétention ; protège du ransomware et des erreurs. **Backup policies** d'Organizations déploient les plans sur tous les comptes.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Backups multi-service centralisés → AWS Backup ; immutabilité → Vault Lock.

## Pièges d'examen

- Sauvegardes que personne ne peut supprimer = AWS Backup Vault Lock.

## Voir aussi

[[dr-et-multi-region]], [[ebs]], [[dynamodb]], [[organizations-et-scp]], [[s3]]
