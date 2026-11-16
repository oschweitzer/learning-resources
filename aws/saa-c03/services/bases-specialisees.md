---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "13. Choosing a database"
---

# Bases spécialisées : DocumentDB, Neptune, Keyspaces, Timestream, MemoryDB

## À quoi ça sert

Associer une charge de travail à la bonne base purpose-built (relationnel, clé-valeur, document, graphe, time series, colonnes larges, in-memory).

## Concepts clés

- Questions à se poser : lecture/écriture ? débit variable ? volume et croissance ? durabilité, source de vérité ? latence, concurrence ? modèle de données et requêtes ? coûts de licences (migrer vers Aurora ?).
- Cartographie : relationnel (RDS, Aurora) ; clé-valeur/document (DynamoDB, DocumentDB) ; in-memory (ElastiCache, MemoryDB, DAX) ; colonnes larges (Keyspaces) ; graphe (Neptune) ; time series (Timestream) ; recherche (OpenSearch) ; data warehouse (Redshift, Athena, EMR) ; objets (S3, Glacier).
- **DocumentDB** : ce qu'Aurora est à MySQL/PostgreSQL, DocumentDB l'est à **MongoDB** ; JSON ; 3 AZ ; stockage par pas de 10 GB ; millions de req/s ; elastic clusters (sharding des écritures).
- **Neptune** : graphe pour données très connectées (réseaux sociaux, knowledge graphs, fraude, recommandations) ; 3 AZ, jusqu'à 15 read replicas ; milliards de relations en ms ; **Neptune Streams** (changements ordonnés, sans doublons, via API HTTP REST).
- **Keyspaces** : Cassandra managé serverless (CQL), 3 réplicas sur AZ, ms à un chiffre ; on-demand ou provisionné + auto scaling ; PITR jusqu'à 35 jours ; IoT, time series, migration Cassandra.
- **Timestream** : time series (SQL, scheduled queries, multi-measure) ; données récentes en mémoire, historique en tier magnétique moins cher ; ingestion IoT, Kinesis, MSK, Prometheus, Telegraf, Lambda. **Statut** : Timestream for LiveAnalytics fermé aux nouveaux clients depuis le 20 juin 2025 → Timestream for InfluxDB recommandé ; pour l'examen « time series » = Timestream.
- Résumés : RDS (SQL/OLTP), Aurora (RDS en mieux), ElastiCache (cache, sessions, leaderboards), DynamoDB (serverless, patterns d'accès connus), S3 (gros objets, pas des milliers de tout petits).

## Limites et quotas

- DocumentDB : stockage par pas de 10 GB ; Neptune : jusqu'à 15 read replicas ; Keyspaces : PITR jusqu'à 35 jours.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- MongoDB → DocumentDB ; Cassandra → Keyspaces ; graphe → Neptune ; time series → Timestream ; recherche sur n'importe quel champ → [[opensearch-emr-et-quick]] ; analytique pétaoctets → [[redshift]].

## Pièges d'examen

- MongoDB = DocumentDB.
- Cassandra = Keyspaces.
- Relations/graphe = Neptune.
- Recherche sur tout champ = OpenSearch, souvent alimenté par DynamoDB Streams.
- Analytique pétaoctets = Redshift, pas RDS.

## Voir aussi

[[dynamodb]], [[rds]], [[aurora]], [[elasticache]], [[redshift]], [[opensearch-emr-et-quick]]
