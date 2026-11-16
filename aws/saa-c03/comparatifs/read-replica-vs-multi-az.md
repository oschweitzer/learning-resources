---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "05. Relational databases and caching"
---

# Read replica vs Multi-AZ (RDS)

Ils ne sont pas interchangeables : l'un scale les lectures, l'autre assure la disponibilité.

|  | Read replicas | Multi-AZ (instance) |
| --- | --- | --- |
| But | Scale des lectures | Disponibilité et DR |
| Réplication | Asynchrone, éventuellement cohérente | Synchrone |
| Emplacement | Même AZ, cross-AZ ou cross-région | Standby dans une autre AZ de la même région |
| Lisible | Oui (endpoint propre) | Non (le standby ne fait que prendre le relais) |
| Failover | Promotion manuelle | Automatique, même DNS |
| Nombre | Jusqu'à 15 | Un standby |

## Règle de décision

- Reporting/analytique lourde sans ralentir la prod → read replica.
- Résister à la perte d'une AZ → Multi-AZ.
- Les deux → read replica en Multi-AZ.

## Pièges d'examen

- Multi-AZ standby non lisible (sauf Multi-AZ DB cluster).

## Voir aussi

[[rds]], [[aurora]]
