---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "10. Decoupling"
---

# Kinesis Data Streams et Amazon Data Firehose

## À quoi ça sert

Collecter et traiter des flux de données en temps réel (Data Streams) ou les charger dans une destination (Firehose).

## Concepts clés

- **Kinesis Data Streams** : clickstreams, IoT, logs, métriques. Rétention jusqu'à **365 jours**, rejouable, données non supprimables avant expiration. Enregistrements jusqu'à 10 MiB (typiquement petits). Ordre garanti par **partition key** → shard. KMS au repos, HTTPS en transit.
- Producteurs : SDK, KPL, Kinesis Agent. Consommateurs : KCL, SDK, Lambda, Firehose, Managed Service for Apache Flink.
- Modes de capacité : *Provisioned* (shards ; 1 MB/s ou 1 000 rec/s en entrée, 2 MB/s en sortie par shard ; split/merge ; facturé par shard-heure) et *On-demand* (démarre à 4 MB/s ou 4 000 rec/s, scale sur le pic des 30 derniers jours ; facturé par stream-heure + GB in/out).
- **Amazon Data Firehose** (ex Kinesis Data Firehose) : managé, serverless, scale auto. Charge S3, Redshift (via S3), OpenSearch, Splunk, Datadog, New Relic, MongoDB, HTTP custom. **Near real-time** (buffer taille/temps). Transformation Lambda, conversion Parquet/ORC, compression, backup S3 des données ou des échecs. Sources : Data Streams, PUT direct, CloudWatch Logs/Events, IoT, SNS.
- Data Streams vs Firehose : code producteur/consommateur vs sans code ; temps réel vs near real-time ; provisionné/on-demand vs scale auto ; stockage 365 j + replay vs pas de stockage ni replay.

## Limites et quotas

- Record jusqu'à 10 MiB.
- Rétention jusqu'à 365 jours.
- Shard provisionné : 1 MB/s ou 1 000 rec/s en entrée, 2 MB/s en sortie.
- On-demand : démarre à 4 MB/s ou 4 000 rec/s.

## Tarification (logique)

- Data Streams provisionné : par shard-heure ; on-demand : par stream-heure + GB in/out.
- Firehose : paiement à l'usage.

## Quand l'utiliser vs alternatives

- Comparatif → [[kinesis-streams-vs-firehose]] et [[sqs-vs-sns-vs-kinesis]]. Kafka → [[streaming-analytique]].

## Pièges d'examen

- Replay ou plusieurs consommateurs du même flux temps réel = Data Streams.
- Charger S3/Redshift sans code = Firehose.

## Voir aussi

[[kinesis-streams-vs-firehose]], [[sqs-vs-sns-vs-kinesis]], [[streaming-analytique]], [[s3]], [[redshift]], [[lambda]]
