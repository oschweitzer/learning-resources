---
tags:
  - aws
  - saa-c03
  - comparatif
  - cost
statut: a-revoir
source: notion
module-notion: "07. Amazon S3"
---

# Classes de stockage S3

Choisir la classe selon la fréquence d'accès, la durée et la résilience.

| Classe | Disponibilité | AZ | Durée min. | Retrieval | Usage |
| --- | --- | --- | --- | --- | --- |
| Standard | 99,99 % | ≥ 3 | Aucune | ms, sans frais | Accès fréquent |
| Intelligent-Tiering | 99,9 % | ≥ 3 | Aucune | ms (tiers fréquent/IA), sans frais | Accès inconnu ou changeant |
| Standard-IA | 99,9 % | ≥ 3 | 30 j | ms, frais/GB | Backups, copies DR |
| One Zone-IA | 99,5 % | 1 | 30 j | ms, frais/GB | Copies secondaires, recréables |
| Glacier Instant Retrieval | 99,9 % | ≥ 3 | 90 j | ms | Archives lues ~1 fois par trimestre |
| Glacier Flexible Retrieval | 99,99 % | ≥ 3 | 90 j | Expedited 1-5 min, Standard 3-5 h, Bulk 5-12 h (gratuit) | Archives, retrieval relâché |
| Glacier Deep Archive | 99,99 % | ≥ 3 | 180 j | Standard 12 h, Bulk 48 h | Conformité long terme, le moins cher |
| Express One Zone | 99,95 % | 1 (directory bucket) | Aucune | ms à un chiffre | Latence critique, ML, HPC |

## Règle de décision

- Accès inconnu → Intelligent-Tiering.
- Archive rarement lue, 48 h OK → Deep Archive (Bulk).
- Donnée recréable → One Zone-IA.
- Latence mini colocalisée avec le compute → Express One Zone.

## Pièges d'examen

- Durées minimales : coûteux pour des données de courte vie.
- Storage Class Analysis ne recommande que Standard → Standard-IA.

## Voir aussi

[[s3]]
