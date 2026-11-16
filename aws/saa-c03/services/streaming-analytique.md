---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "14. Data and analytics"
---

# Managed Service for Apache Flink et Amazon MSK

## À quoi ça sert

Analytique de flux (Flink) et Kafka managé (MSK), en alternative/complément de Kinesis.

## Concepts clés

- **Managed Service for Apache Flink** (ex Kinesis Data Analytics for Flink) : applications Flink (Java, Scala, SQL) sur cluster managé auto-scalé, checkpoints et snapshots. Lit Kinesis Data Streams ou MSK, **pas Firehose**.
- **Amazon MSK** : Apache Kafka managé (brokers dans votre VPC sur jusqu'à 3 AZ, reprise sur panne, données sur EBS aussi longtemps que voulu). **MSK Serverless** : sans gérer la capacité. Consommateurs : Flink, Glue streaming ETL, Lambda, EC2/ECS/EKS.
- Pipeline de référence : IoT Core → Kinesis Data Streams → Firehose (toutes les ~minutes) → S3 → SQS/Lambda → requêtes Athena → Quick, et Redshift pour l'entreposage.

## Limites et quotas

- Kinesis : records jusqu'à 10 MiB, rétention 365 j. MSK : 1 MB par défaut (configurable), rétention tant que vous payez le stockage.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Kafka existant/portable → MSK ; flux AWS natif → [[kinesis-et-firehose]]. Comparatif → [[kinesis-vs-msk]].

## Pièges d'examen

- Kafka = MSK.
- Flink ne lit pas Firehose.

## Voir aussi

[[kinesis-et-firehose]], [[kinesis-vs-msk]], [[athena]], [[redshift]]
