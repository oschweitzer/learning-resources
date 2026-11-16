---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "16. Monitoring and audit"
---

# AWS Config

## À quoi ça sert

Enregistrer la configuration des ressources et ses changements dans le temps ; vérifier la conformité. Répond à « quelle config, comment a-t-elle changé, est-elle conforme ? ».

## Concepts clés

- Par région (agrégable cross-région/compte), snapshots dans S3. Pour chaque ressource : conformité dans le temps, config dans le temps, appels CloudTrail à l'origine du changement. Notifications SNS lors de changements.
- Questions : SSH ouvert à tous dans mes SG ? buckets publics ? comment cet ALB a-t-il changé ?
- **Config rules** : managées ou custom (Lambda, Guard) ; évaluées à chaque changement et/ou périodiquement. Ex. EBS tous en gp3 ? types d'instance autorisés ? certificat bientôt expiré (`acm-certificate-expiration-check`) ? **Ne bloquent rien** (pas de deny) → pour bloquer : SCP ou IAM.
- **Remédiation** via documents SSM Automation (managés ou custom, pouvant appeler Lambda), avec retries ; ex. révoquer les access keys de plus de 90 jours. Notifications EventBridge en cas de non-conformité.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Facturé par configuration item enregistré et par évaluation de rule.

## Quand l'utiliser vs alternatives

- Conformité/historique de configuration → Config ; bloquer → [[organizations-et-scp]]. Comparatif → [[cloudwatch-vs-cloudtrail-vs-config]].

## Pièges d'examen

- Config rules ne bloquent jamais une action.
- Conforme ? = Config.

## Voir aussi

[[cloudtrail]], [[cloudwatch]], [[systems-manager]], [[control-tower]], [[cloudwatch-vs-cloudtrail-vs-config]]
