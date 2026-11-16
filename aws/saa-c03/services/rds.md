---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - secure
statut: a-revoir
source: notion
module-notion: "05. Relational databases and caching"
---

# Amazon RDS (et RDS Proxy)

## À quoi ça sert

Bases relationnelles SQL managées : provisioning, patch OS, backups, monitoring, read replicas, Multi-AZ, maintenance windows.

## Concepts clés

- Moteurs : PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, IBM Db2, Aurora. Stockage sur EBS. Pas de SSH (sauf **RDS Custom**).
- **Storage Auto Scaling** : agrandit jusqu'à un maximum fixé quand l'espace libre < 10 % pendant ≥ 5 min et que le dernier changement date de plus de 6 h.
- **Read replicas** : scale des lectures, asynchrone, même AZ / cross-AZ / cross-région, jusqu'à 15, endpoint propre (l'appli doit l'utiliser), SELECT seulement, promotion manuelle. Un replica peut lui-même être Multi-AZ.
- **Multi-AZ (instance)** : disponibilité/DR, réplication **synchrone**, un standby non lisible dans une autre AZ, failover automatique, même DNS. Passer de Single-AZ à Multi-AZ = modification sans downtime (snapshot → restauration → réplication).
- **Multi-AZ DB cluster** (MySQL, PostgreSQL) : 1 writer + 2 standbys lisibles sur 3 AZ, failover plus rapide.
- Réseau : réplication inter-AZ dans la même région gratuite pour RDS ; cross-région facturée.
- **RDS Custom** (Oracle, SQL Server) : accès OS et base ; mettre l'automation en pause avant de customiser, snapshot d'abord ; SSH ou Session Manager.
- **Backups** : automatiques = full quotidien + logs de transaction toutes les 5 min, PITR, rétention 1 à 35 jours (0 = désactivé) ; snapshots manuels conservés à volonté. Restaurer **crée toujours une nouvelle base**. Base arrêtée = stockage toujours facturé.
- **Sécurité** : chiffrement KMS **à la création** (si le primary n'est pas chiffré, les replicas non plus). Pour chiffrer l'existant : snapshot → copie chiffrée → restauration. TLS en transit. IAM database authentication (MySQL, PostgreSQL). Credentials dans Secrets Manager (rotation).
- **RDS Proxy** : proxy managé, serverless, Multi-AZ, qui mutualise les connexions ; réduit le failover jusqu'à 66 % ; IAM auth + Secrets Manager ; **jamais public** (dans le VPC). Idéal pour Lambda qui ouvre beaucoup de connexions courtes.
- Migration on-premises MySQL → RDS MySQL : backup dans S3.

## Limites et quotas

- Read replicas : jusqu'à 15.
- Rétention des backups automatiques : 1 à 35 jours.
- Storage Auto Scaling : seuil < 10 % libre pendant 5 min, > 6 h depuis la dernière modif.

## Tarification (logique)

- Réplication inter-AZ gratuite ; cross-région facturée.
- Une base arrêtée paie encore le stockage : pour un usage de quelques heures par mois, snapshot puis restauration.

## Quand l'utiliser vs alternatives

- Read replica vs Multi-AZ → [[read-replica-vs-multi-az]]. Plus de perf / fonctions → [[aurora]]. Cache → [[elasticache]]. NoSQL → [[dynamodb]].
- Lambda + nombreuses connexions → RDS Proxy (la fonction doit être dans le VPC).

## Pièges d'examen

- Read replicas = scale lecture ; Multi-AZ = disponibilité ; non interchangeables.
- Le standby Multi-AZ n'est pas lisible (sauf Multi-AZ DB cluster).
- Le chiffrement se décide à la création.
- Lambda + trop de connexions = RDS Proxy.

## Voir aussi

[[aurora]], [[read-replica-vs-multi-az]], [[elasticache]], [[kms]], [[secrets-manager-et-parameter-store]], [[lambda]], [[dms-et-migration]]
