---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "20. Disaster recovery, migration and multi-region"
---

# AWS DMS, SCT et migration vers Aurora

## À quoi ça sert

Migrer des bases de données vers AWS pendant que la source reste disponible.

## Concepts clés

- **DMS** : migrations homogènes (Oracle → Oracle) ou hétérogènes (SQL Server → Aurora) ; **réplication continue avec CDC** ; tourne sur une *replication instance* (ou DMS Serverless).
- Sources : bases on-premises/EC2 (Oracle, SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, Db2), Azure SQL Database, RDS/Aurora, S3, DocumentDB. Cibles : bases on-premises/EC2, RDS, Aurora, Redshift, DynamoDB, S3, OpenSearch, Kinesis Data Streams, Kafka, DocumentDB, Neptune, Redis.
- **SCT** (Schema Conversion Tool) : convertit les schémas entre moteurs, OLTP (SQL Server/Oracle → MySQL/PostgreSQL/Aurora) et OLAP (Teradata/Oracle → Redshift) ; **inutile si même moteur** ; instances compute-optimized pour la conversion. Pattern : SCT on-premises convertit le schéma, puis DMS (replication instance) fait full load + CDC.
- **DMS Multi-AZ** : réplica standby synchrone dans une autre AZ (redondance, moins de gels d'I/O et de pics de latence).
- **Vers Aurora** : depuis RDS MySQL/PostgreSQL → restaurer un snapshot RDS en cluster Aurora (downtime) ou créer un Aurora read replica de l'instance RDS puis le promouvoir à lag nul (temps et coût) ; MySQL externe → Percona XtraBackup vers S3 puis cluster Aurora MySQL depuis S3 (ou mysqldump, plus lent) ; PostgreSQL externe → backup vers S3 puis import avec l'extension `aws_s3` ; deux bases actives → DMS en réplication continue.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Migration hétérogène → SCT + DMS ; même moteur → DMS (ou outils natifs). Autres outils → [[migration-et-outils]].

## Pièges d'examen

- Hétérogène = SCT + DMS.
- SCT inutile pour le même moteur.

## Voir aussi

[[aurora]], [[rds]], [[migration-et-outils]], [[dynamodb]], [[redshift]]
