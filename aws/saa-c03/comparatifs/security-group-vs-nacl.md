---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# Security group vs NACL

Deux niveaux de filtrage.

| Security group | NACL |
| --- | --- |
| Niveau instance (ENI) | Niveau subnet |
| Règles allow uniquement | Règles allow et deny |
| Stateful : retour automatique | Stateless : retour à autoriser |
| Toutes les règles évaluées ensemble | Évaluées dans l'ordre, première correspondance gagne |
| S'applique aux instances auxquelles il est attaché | S'applique à toutes les instances du subnet |

## Règle de décision

- Bloquer une IP → NACL.
- Autoriser seulement le LB → SG référençant le SG du LB.

## Pièges d'examen

- NACL : ports éphémères.
- SG ne peuvent pas refuser.

## Voir aussi

[[security-groups-et-nacl]]
