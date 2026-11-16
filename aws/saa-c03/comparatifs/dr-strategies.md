---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
statut: a-revoir
source: notion
module-notion: "20. Disaster recovery, migration and multi-region"
---

# Stratégies de DR : backup, pilot light, warm standby, multi-site

Du moins cher au plus cher ; valeurs RPO/RTO = ordres de grandeur.

| Stratégie | Principe | RPO / RTO typiques | Coût |
| --- | --- | --- | --- |
| Backup and restore | Backups copiés (snapshots, AMI, S3…), infra reconstruite après l'incident | Heures | Le plus bas |
| Pilot light | Cœur critique actif (base répliquée), serveurs d'appli prêts mais éteints | Dizaines de minutes | Bas |
| Warm standby | Copie complète à échelle réduite, montée en charge lors de l'incident | Minutes | Moyen |
| Multi-site (active-active) | Pleine échelle à deux endroits qui servent du trafic | Proche de zéro, secondes | Le plus élevé |

## Règle de décision

- Choisir selon RTO/RPO et budget par application.
- Plus c'est court, plus c'est cher.

## Pièges d'examen

- Un plan de DR non testé est inexistant.
- Penser aux quotas de la région de secours.

## Voir aussi

[[dr-et-multi-region]], [[aws-backup]]
