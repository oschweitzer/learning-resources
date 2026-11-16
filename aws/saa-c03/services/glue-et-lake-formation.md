---
tags:
  - aws
  - saa-c03
  - performant
  - secure
statut: a-revoir
source: notion
module-notion: "14. Data and analytics"
---

# AWS Glue et AWS Lake Formation

## À quoi ça sert

ETL serverless pour préparer les données (Glue) et construire/gouverner un data lake avec permissions fines (Lake Formation).

## Concepts clés

- **Glue** : ETL serverless. Pattern classique : événement S3 → Lambda/EventBridge → job Glue ETL CSV → Parquet → requêtes Athena.
- **Glue Data Catalog** : crawlers scannent S3, RDS, DynamoDB, JDBC et écrivent les métadonnées de tables ; utilisé par Athena, Redshift Spectrum, EMR. **Job bookmarks** (éviter de retraiter), **DataBrew** (nettoyage/normalisation), **Glue Studio** (ETL visuel), streaming ETL (Spark Structured Streaming : Kinesis, Kafka, MSK).
- **Lake Formation** : data lake (point central de toutes les données d'analyse) en jours au lieu de mois, sur S3 et le Glue Data Catalog ; découverte, nettoyage, transformation, ingestion (blueprints) ; données structurées et non structurées.
- Contrôle d'accès centralisé, **fin au niveau ligne et colonne**, appliqué à Athena, Redshift, EMR, Quick… ; partage cross-account.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Convertir/transformer → Glue ; sécurité par ligne/colonne sur un data lake → Lake Formation.

## Pièges d'examen

- Sécurité ligne/colonne sur un data lake = Lake Formation.

## Voir aussi

[[athena]], [[redshift]], [[s3]], [[opensearch-emr-et-quick]]
