---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
statut: a-revoir
source: notion
module-notion: "14. Data and analytics"
---

# Kinesis Data Streams vs Amazon MSK

AWS natif ou Kafka.

| Kinesis Data Streams | Amazon MSK |
| --- | --- |
| Records jusqu'à 10 MiB, typiquement petits | 1 MB par défaut, configurable |
| Streams avec shards (split/merge) | Topics avec partitions (ajout seulement) |
| TLS en transit | Plaintext ou TLS |
| KMS au repos | KMS au repos |
| Rétention jusqu'à 365 j | Rétention aussi longue que le stockage payé |
| API AWS | API Kafka, portable |

## Règle de décision

- Besoin de l'API Kafka / portabilité → MSK.
- Intégration AWS native simple → Kinesis.

## Pièges d'examen

- Rien de précis dans mes notes Notion.

## Voir aussi

[[kinesis-et-firehose]], [[streaming-analytique]]
