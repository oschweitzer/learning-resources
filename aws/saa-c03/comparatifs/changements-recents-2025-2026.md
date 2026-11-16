---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
  - secure
  - cost
statut: a-revoir
source: notion
module-notion: "Page racine : What changed recently"
---

# Changements récents à connaître (jusqu'en novembre 2026)

Plus récents que beaucoup de supports de formation. L'examen peut être en retard sur les services : connaître la réponse classique et l'état actuel.

| Date | Changement | Note |
| --- | --- | --- |
| Juin 2025 | DynamoDB global tables : multi-region strong consistency (RPO zéro) | [[dynamodb]] |
| Juin 2025 | Timestream for LiveAnalytics fermé aux nouveaux clients ; InfluxDB recommandé | [[bases-specialisees]] |
| Août 2025 | SQS : message max 256 KiB → 1 MiB | [[sqs]] |
| Septembre 2025 | EBS gp3 jusqu'à 64 TiB, 80 000 IOPS, 2 000 MiB/s | [[ebs]] |
| Octobre 2025 | QuickSight → Amazon Quick Suite, puis Amazon Quick | [[opensearch-emr-et-quick]] |
| Novembre 2025 | Snowball Edge et S3 Object Lambda fermés aux nouveaux clients | [[transfert-de-donnees]], [[s3]] |
| Novembre 2025 | NAT gateway régionale | [[nat]] |
| Décembre 2025 | S3 : taille max d'objet 5 TB → 50 TB | [[s3]] |
| Décembre 2025 | Glacier autonome (vaults) fermé aux nouveaux clients ; classes S3 Glacier inchangées | [[s3]] |
| Avril 2026 | Amazon S3 Files : bucket S3 monté comme file system partagé | [[fsx-et-s3-files]] |
| Octobre 2026 | Fin de support d'Amazon Pinpoint | [[services-divers]] |
| Octobre 2026 | Exam guide 1.1 : Elastic Transcoder retiré du scope | [[services-divers]] |

## Règle de décision

- Apprendre la réponse classique ET l'état actuel du service.
- Lambda accepte aussi des payloads asynchrones jusqu'à 1 MB ([[lambda]]).

## Pièges d'examen

- Rien de précis dans mes notes Notion.

## Voir aussi

[[s3]], [[sqs]], [[ebs]], [[nat]], [[dynamodb]]
