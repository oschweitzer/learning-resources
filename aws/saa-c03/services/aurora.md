---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "05. Relational databases and caching"
---

# Amazon Aurora

## À quoi ça sert

Moteur AWS compatible MySQL/PostgreSQL : plus de performance et de flexibilité que RDS.

## Concepts clés

- Jusqu'à 5x MySQL sur RDS et 3x PostgreSQL sur RDS (selon AWS). Coûte environ 20 % de plus que RDS mais plus efficace.
- Stockage auto de 10 GB en 10 GB jusqu'à 256 TiB. **6 copies sur 3 AZ** : écriture = 4 sur 6, lecture = 3 sur 6 ; self-healing ; striping sur des centaines de volumes.
- 1 writer + jusqu'à **15 replicas**, failover < 30 s, replicas cross-région et auto scaling.
- **Endpoints** : *writer/cluster* (toujours le primary), *reader* (load-balance sur les replicas), *custom* (sous-ensemble, ex. gros replicas pour l'analytique).
- **Aurora Serverless v2** : capacité auto selon la charge, facturation à la seconde ; usage rare/intermittent/imprévisible.
- **Global Database** : 1 région primaire (R/W) + jusqu'à **10** régions secondaires en lecture, jusqu'à 16 replicas par région secondaire, lag < 1 s, **RTO < 1 min** pour promotion DR ; réduit la latence de lecture des utilisateurs distants.
- **Cloning** : nouveau cluster en copy-on-write, plus rapide que snapshot/restore ; staging depuis la prod.
- **Backtrack** (Aurora MySQL) : remonter dans le temps sans restaurer.
- **Machine learning** : appel de SageMaker AI ou Comprehend depuis SQL. **Babelfish** : Aurora PostgreSQL comprend T-SQL (migration SQL Server). **I/O-Optimised** : pas de frais par I/O.
- Backups : rétention 1 à 35 jours, **ne peut pas être désactivée**, PITR. Migration MySQL externe : Percona XtraBackup → S3.

## Limites et quotas

- Stockage jusqu'à 256 TiB ; jusqu'à 15 replicas ; Global Database : 10 régions secondaires, 16 replicas/région.
- Backups : 1 à 35 jours, non désactivables.

## Tarification (logique)

- ~20 % plus cher que RDS mais plus efficace.
- Serverless v2 : facturation à la seconde.
- I/O-Optimised supprime les frais par I/O (utile pour charges très I/O).

## Quand l'utiliser vs alternatives

- Besoin de plus de perf ou de fonctions que [[rds]] → Aurora. Cross-région avec RPO ~1 s et RTO < 1 min → Global Database. Usage sporadique → Serverless v2.

## Pièges d'examen

- DR cross-région avec RPO d'environ une seconde et RTO < 1 min = Aurora Global Database.
- Staging rapide depuis la prod = cloning, pas snapshot/restore.

## Voir aussi

[[rds]], [[dms-et-migration]], [[dr-et-multi-region]], [[dynamodb]]
