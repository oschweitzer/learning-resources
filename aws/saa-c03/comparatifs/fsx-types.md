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

# Les 4 FSx

Choisir le bon file system FSx.

| FSx | Protocoles | Forces | Usages |
| --- | --- | --- | --- |
| Windows File Server | SMB, NTFS | AD, ACL, quotas, DFS, Multi-AZ, backups S3 | Partages Windows, SharePoint, lift & shift |
| Lustre | Lustre (POSIX) | Centaines de GB/s, millions d'IOPS, lié à S3 ; scratch ou persistent | HPC, ML, vidéo, finance, EDA |
| NetApp ONTAP | NFS, SMB, iSCSI | Snapshots, réplication, compression, dédup, clonage ; Linux/Windows/macOS/VMware | Migration NetApp/NAS |
| OpenZFS | NFS v3 à v4.2 | Jusqu'à 1 M d'IOPS sub-ms, snapshots, clonage instantané | Migration ZFS, clones dev/test |

## Règle de décision

- Windows + AD → Windows File Server.
- HPC + S3 → Lustre.
- NAS NetApp / multi-protocoles → ONTAP.
- ZFS → OpenZFS.

## Pièges d'examen

- Rien de précis dans mes notes Notion.

## Voir aussi

[[fsx-et-s3-files]]
