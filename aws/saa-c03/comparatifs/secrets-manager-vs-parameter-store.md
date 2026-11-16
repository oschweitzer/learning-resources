---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
statut: a-revoir
source: notion
module-notion: "18. Security and encryption"
---

# Secrets Manager vs Parameter Store

Rotation intégrée ou stockage de configuration économique.

|  | Secrets Manager | Parameter Store |
| --- | --- | --- |
| Rotation | Automatique via Lambda, natif RDS/Aurora/Redshift/DocumentDB | Pas de rotation native (parameter policies advanced : TTL/notifications) |
| Multi-région | Secrets multi-région avec réplicas promouvables | — |
| Coût | Par secret + appels API | Standard gratuit ; advanced par paramètre |
| Taille | — | 4 KB standard, 8 KB advanced |

## Règle de décision

- Rotation de mots de passe de base → Secrets Manager.
- Config pas chère, hiérarchique → Parameter Store.

## Pièges d'examen

- Rien de précis dans mes notes Notion.

## Voir aussi

[[secrets-manager-et-parameter-store]]
