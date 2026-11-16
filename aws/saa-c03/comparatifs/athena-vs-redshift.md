---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "14. Data and analytics"
---

# Athena vs Redshift

Requêter S3 sur demande ou entreposer pour de l'analytique lourde.

|  | Athena | Redshift |
| --- | --- | --- |
| Modèle | Serverless, SQL sur fichiers S3, sans chargement | Data warehouse (PostgreSQL), stockage colonnes, MPP, données chargées |
| Facturation | Au TB scanné | Cluster provisionné ou Serverless |
| Idéal pour | Requêtes ponctuelles, logs (Flow Logs, ELB, CloudTrail) | Jointures, agrégations, requêtes complexes répétées |
| Pont | — | Spectrum interroge S3 sans charger |

## Règle de décision

- Occasionnel sur S3 → Athena.
- Analytique lourde répétée → Redshift.

## Pièges d'examen

- Athena : Parquet + compression + partitions pour réduire le coût.

## Voir aussi

[[athena]], [[redshift]]
