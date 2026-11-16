---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
  - resilient
statut: a-revoir
source: notion
module-notion: "16. Monitoring and audit"
---

# CloudWatch vs CloudTrail vs Config

Quel outil répond à quelle question.

| Outil | Répond à | Pour un load balancer |
| --- | --- | --- |
| CloudWatch | Comment ça se comporte ? Alertez-moi. Agrégation de logs | Connexions entrantes, codes d'erreur dans le temps, dashboard de perf |
| CloudTrail | Qui a appelé quelle API, quand, d'où ? | Qui a modifié le LB via l'API |
| Config | Quelle configuration, comment a-t-elle changé, est-elle conforme ? | Règles de SG et changements de config ; certificat TLS toujours attaché |

## Règle de décision

- Who did what → CloudTrail.
- Is it compliant → Config.
- Is it healthy → CloudWatch.

## Pièges d'examen

- Config rules ne bloquent pas.
- CloudTrail event history = 90 jours.

## Voir aussi

[[cloudwatch]], [[cloudtrail]], [[config]]
