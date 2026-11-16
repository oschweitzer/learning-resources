---
tags:
  - aws
  - saa-c03
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "14. Data and analytics"
---

# Amazon Athena

## À quoi ça sert

Requêtes SQL serverless directement sur des fichiers dans S3 (CSV, JSON, ORC, Avro, Parquet), basé sur Trino/Presto.

## Concepts clés

- Usages : ad hoc, BI, reporting, analyse des VPC Flow Logs, ELB logs, CloudTrail logs. Souvent associé à Amazon Quick pour les dashboards.
- **Réduire le coût/accélérer** : formats colonnes (Parquet, ORC ; Glue peut convertir), compression (gzip, snappy, zstd…), **partitionnement** (s3://bucket/year=2026/month=11/day=03/), moins de fichiers plus gros (> 128 MB).
- **Federated queries** : data source connectors (fonctions Lambda) vers relationnel, NoSQL, objet, sources custom, sur AWS ou on-premises (CloudWatch Logs, DynamoDB, RDS, DocumentDB, Redshift). Résultats stockés dans S3.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Facturé au TB scanné (d'où colonnes, compression, partitions).

## Quand l'utiliser vs alternatives

- Requêtes occasionnelles sur S3 sans chargement → Athena ; jointures/agrégations lourdes et répétées → [[redshift]]. Comparatif → [[athena-vs-redshift]].

## Pièges d'examen

- SQL sur S3 sans serveur = Athena.
- Rendre Athena moins cher = Parquet, compression, partitions.

## Voir aussi

[[redshift]], [[athena-vs-redshift]], [[glue-et-lake-formation]], [[s3]], [[opensearch-emr-et-quick]]
