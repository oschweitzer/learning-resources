---
tags:
  - aws
  - saa-c03
  - performant
statut: a-revoir
source: notion
module-notion: "14. Data and analytics"
---

# Amazon Redshift

## À quoi ça sert

Data warehouse basé sur PostgreSQL, pour l'OLAP (pas l'OLTP) : stockage colonnes, moteur massivement parallèle, pétaoctets.

## Concepts clés

- Deux modes : **provisionné** (types de nœuds, réservable) et **Serverless**. Cluster = leader node (planification, agrégation) + compute nodes.
- Plus rapide qu'Athena pour jointures, agrégations et requêtes complexes répétées (données chargées, index) ; Athena mieux pour du ponctuel sans chargement.
- **Snapshots / DR** : Multi-AZ pour certains types de nœuds ; snapshots incrémentaux dans S3 ; restauration = **nouveau cluster** ; automatiques toutes les 8 h, tous les 5 GB ou planifiés ; manuels conservés ; copie auto vers une autre région.
- **Chargement** : Firehose → S3 puis `COPY` ; `COPY` depuis S3 avec IAM role (idéalement **enhanced VPC routing**) ; inserts JDBC possibles mais à grouper.
- **Redshift Spectrum** : interroge S3 sans charger, depuis un cluster existant, avec des milliers de nœuds Spectrum.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Provisionné : réserver pour économiser ; Serverless disponible.

## Quand l'utiliser vs alternatives

- Analytique lourde et répétée → Redshift. Requêter S3 depuis Redshift sans charger → Spectrum. Occasionnel sur S3 → [[athena]].

## Pièges d'examen

- Analytique complexe répétée = Redshift.
- Query S3 depuis Redshift sans charger = Spectrum.

## Voir aussi

[[athena]], [[athena-vs-redshift]], [[kinesis-et-firehose]], [[s3]], [[glue-et-lake-formation]]
