---
tags:
  - aws
  - saa-c03
  - performant
statut: a-revoir
source: notion
module-notion: "14. Data and analytics"
---

# OpenSearch Service, EMR et Amazon Quick

## À quoi ça sert

Recherche/analytique de logs (OpenSearch), big data Hadoop/Spark (EMR) et BI serverless (Quick).

## Concepts clés

- **OpenSearch Service** (successeur d'Elasticsearch Service) : recherche sur **n'importe quel champ**, y compris partielle (DynamoDB ne requête que par clé/index) ; complément d'une autre base ; analytique de logs avec dashboards ; clusters managés ou serverless ; ingestion Firehose, IoT, CloudWatch Logs ; sécurité Cognito, IAM, KMS, TLS.
- Patterns OpenSearch : DynamoDB → Streams → Lambda → OpenSearch (recherche, puis fetch de l'item complet dans DynamoDB) ; CloudWatch Logs subscription filter → Lambda (temps réel) ou Firehose (near real-time) → OpenSearch ; Kinesis Data Streams → Firehose ou Lambda → OpenSearch.
- **EMR** : clusters Hadoop managés (centaines d'EC2) avec Spark, HBase, Presto, Flink ; provisioning, config, auto scaling, intégration Spot. Aussi sur EKS et en serverless. **Nœuds** : primary (cluster, long-running : On-Demand/Reserved), core (tâches + données HDFS, long-running : On-Demand/Reserved), task (tâches seulement, sans stockage : **Spot**). Clusters long-running ou transitoires.
- **Amazon Quick** (ex QuickSight) : BI serverless ML avec dashboards interactifs ; facturé par utilisateur ou session. Octobre 2025 : QuickSight devient Amazon Quick Suite puis Amazon Quick en 2026 (la partie BI = Quick Sight) ; le guide d'examen dit Amazon Quick. Sources : RDS, Aurora, Athena, Redshift, S3, OpenSearch, Timestream, SaaS (Salesforce, Jira), on-premises via JDBC, fichiers. **SPICE** = moteur en mémoire (données importées). L'édition Enterprise ajoute la sécurité au niveau colonne. Utilisateurs/groupes Quick **hors IAM** ; un dashboard est un snapshot en lecture seule d'une analyse.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- EMR : Spot recommandé pour les nœuds task.

## Quand l'utiliser vs alternatives

- Recherche plein texte → OpenSearch ; big data Spark/Hadoop → EMR ; dashboards → Quick.

## Pièges d'examen

- Recherche sur n'importe quel champ = OpenSearch.
- Nœuds EMR task = Spot.
- Les utilisateurs Quick ne sont pas des users IAM.

## Voir aussi

[[dynamodb]], [[athena]], [[redshift]], [[glue-et-lake-formation]], [[bases-specialisees]]
