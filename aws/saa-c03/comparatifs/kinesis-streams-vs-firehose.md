---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
statut: a-revoir
source: notion
module-notion: "10. Decoupling"
---

# Kinesis Data Streams vs Amazon Data Firehose

Collecter un flux ou le charger dans une destination.

| Kinesis Data Streams | Amazon Data Firehose |
| --- | --- |
| Collecte des flux, vous écrivez producteurs et consommateurs | Charge des flux dans des destinations, sans code |
| Temps réel | Near real-time (buffering) |
| Provisionné ou on-demand | Scaling entièrement automatique |
| Stockage jusqu'à 365 j, replay possible | Pas de stockage ni de replay |

## Règle de décision

- Besoin de replay ou de plusieurs consommateurs → Data Streams.
- Charger S3/Redshift/OpenSearch sans code → Firehose.

## Pièges d'examen

- Flink ne lit pas Firehose.

## Voir aussi

[[kinesis-et-firehose]]
