---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
statut: a-revoir
source: notion
module-notion: "09. Storage extras and hybrid storage"
---

# Types de Storage Gateway

Quelle gateway pour quel besoin hybride.

| Gateway | Interface on-premises | Adossé à | Usage |
| --- | --- | --- | --- |
| S3 File Gateway | NFS, SMB | S3 (+ Glacier par lifecycle) | Partages adossés à S3 avec cache local |
| Volume Gateway cached | iSCSI | S3 + snapshots EBS | Données récentes en cache, le reste dans AWS |
| Volume Gateway stored | iSCSI | Dataset complet on-premises, backups S3 (snapshots EBS) | Données primaires locales, backups cloud |
| Tape Gateway | iSCSI VTL | S3 et Glacier | Garder le logiciel/processus de backup à bandes |

## Règle de décision

- Fichiers → S3 File Gateway.
- Blocs → Volume Gateway.
- Bandes → Tape Gateway.

## Pièges d'examen

- Rien de précis dans mes notes Notion.

## Voir aussi

[[storage-gateway]]
