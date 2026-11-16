---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
statut: a-revoir
source: notion
module-notion: "06. Route 53"
---

# CNAME vs Alias (Route 53)

Deux façons de faire pointer un nom vers un autre.

|  | CNAME | Alias |
| --- | --- | --- |
| Pointe vers | N'importe quel hostname | Une ressource AWS |
| Zone apex | Interdit | Autorisé |
| Coût | Requêtes facturées | Requêtes gratuites |
| Health check | Possible sur le record | Évaluation native de la santé de la cible |
| Type de record | CNAME | A ou AAAA |

## Règle de décision

- Apex vers LB/CloudFront → alias.
- Cible hors AWS (SaaS) → CNAME (sur un sous-domaine).
- Un CNAME est seul sur son nom (pas de TXT/MX avec).

## Pièges d'examen

- example.com CNAME other.com est interdit (l'apex porte SOA et NS).
- Pas d'alias vers un DNS public EC2.

## Voir aussi

[[route-53]]
