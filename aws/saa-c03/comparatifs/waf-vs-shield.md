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

# WAF vs Shield vs Firewall Manager

Applicatif L7, DDoS L3/L4 et gestion centralisée.

| Service | Protège contre | Remarque |
| --- | --- | --- |
| WAF | Exploits web L7 (SQL injection, XSS), IP, geo, rate-based | Pas sur NLB ; web ACL régionale sauf CloudFront |
| Shield Standard | DDoS L3/L4 | Gratuit, automatique |
| Shield Advanced | DDoS sophistiqués sur EC2, ELB, CloudFront, GA, Route 53 | SRT 24/7, protection des coûts de scaling, règles WAF automatiques |
| Firewall Manager | Gère WAF, Shield Advanced, SG, Network Firewall, DNS Firewall à l'échelle de l'organisation | S'applique aux nouvelles ressources/comptes |

## Règle de décision

- SQL injection / XSS → WAF.
- DDoS support + protection de coûts → Shield Advanced.
- Multi-comptes → Firewall Manager.

## Pièges d'examen

- SQL injection ≠ Shield.

## Voir aussi

[[waf-shield-firewall-manager]]
