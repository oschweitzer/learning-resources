---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "03, 07, 09"
---

# EBS vs EFS vs S3 (et autres stockages)

Quel stockage pour quel besoin.

| Service | Type | En une ligne |
| --- | --- | --- |
| S3 | Objet | Stockage d'objets durable pour tout |
| S3 Glacier | Objet archive | Archives long terme au moindre coût |
| EBS | Bloc | Disque réseau pour une instance (Multi-Attach mis à part), une AZ |
| Instance store | Bloc | Physique, très rapide, éphémère |
| EFS | Fichier (NFS) | POSIX partagé Linux, multi-AZ |
| FSx for Windows | Fichier (SMB) | Partages Windows avec AD |
| FSx for Lustre | Fichier | Parallèle HPC lié à S3 |
| FSx for NetApp ONTAP | Fichier et bloc | Compatibilité protocolaire la plus large |
| FSx for OpenZFS | Fichier (NFS) | ZFS managé |
| S3 Files | Fichier sur S3 | Accès fichier à un bucket existant |
| Storage Gateway | Hybride | Accès on-premises à S3, snapshots EBS, bandes virtuelles |
| Transfer Family | Transfert | FTP/FTPS/SFTP vers S3 ou EFS |
| DataSync | Transfert | Synchronisation en ligne planifiée, garde les métadonnées |
| Snowball Edge | Transfert | Migration offline et edge compute (clients existants) |

## Règle de décision

- Disque persistant pour une instance → EBS.
- IOPS local maximal, perte acceptable → instance store.
- POSIX partagé Linux multi-AZ → EFS.
- Windows/Lustre/ONTAP/ZFS → FSx.
- Objets, data lake, statique → S3.
- Fichiers sur des données déjà dans S3 → S3 Files.

## Pièges d'examen

- EBS lié à une AZ.
- EFS pas pour Windows.

## Voir aussi

[[ebs]], [[efs]], [[s3]], [[fsx-et-s3-files]], [[ami-et-instance-store]]
