---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "10. Decoupling"
---

# SQS vs SNS vs Kinesis Data Streams

File, pub/sub ou flux : quel modèle de découplage.

|  | SQS | SNS | Kinesis Data Streams |
| --- | --- | --- | --- |
| Modèle | Les consommateurs tirent (pull) | Poussé vers de nombreux abonnés | Pull (standard) ou push (enhanced fan-out) |
| Persistance | Jusqu'à suppression (14 j max) | Aucune, perdu si non livré | Jusqu'à 365 j, rejouable |
| Consommateurs | Workers partageant une file | Jusqu'à 12,5 M d'abonnements par topic | Plusieurs applis lisant les mêmes données |
| Ordre | FIFO uniquement | FIFO topics uniquement | Par shard |
| Débit | Sans provisioning (standard) | Sans provisioning | Shards provisionnés ou on-demand |
| Usage typique | Découpler et tamponner | Notifier et fan-out | Analytique temps réel, flux ordonnés, replay |

## Règle de décision

- Tampon de travail → SQS.
- Un événement → plusieurs consommateurs → SNS + SQS fan-out.
- Replay / multi-consommateurs d'un flux → Kinesis.
- AMQP/MQTT existant → Amazon MQ.

## Pièges d'examen

- Doublons de traitement : visibility timeout + idempotence.

## Voir aussi

[[sqs]], [[sns]], [[kinesis-et-firehose]], [[amazon-mq]]
