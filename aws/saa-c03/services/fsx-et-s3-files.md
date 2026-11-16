---
tags:
  - aws
  - saa-c03
  - performant
statut: a-revoir
source: notion
module-notion: "09. Storage extras and hybrid storage"
---

# Amazon FSx et Amazon S3 Files

## À quoi ça sert

File systems tiers managés (FSx) et accès fichier à un bucket S3 existant (S3 Files, avril 2026).

## Concepts clés

- **FSx for Windows File Server** : SMB/NTFS, intégration Active Directory, ACL, quotas, DFS namespaces, Multi-AZ, backups quotidiens vers S3, SSD ou HDD ; montable aussi depuis Linux. Usages : partages Windows, home directories, SharePoint, lift & shift.
- **FSx for Lustre** : parallèle, POSIX, centaines de GB/s, millions d'IOPS, sub-ms ; lit S3 comme un file system et y écrit les résultats. HPC, ML, vidéo, finance, EDA. *Scratch* : temporaire, non répliqué, burst très élevé ; *Persistent* : long terme, répliqué dans l'AZ.
- **FSx for NetApp ONTAP** : NFS, SMB, iSCSI ; snapshots, réplication, compression, déduplication, clonage ; Linux, Windows, macOS, VMware Cloud on AWS. Migration NetApp/NAS.
- **FSx for OpenZFS** : NFS v3 à v4.2, jusqu'à 1 M d'IOPS sub-ms, snapshots, compression, clonage instantané. Migration ZFS, clones dev/test.
- **S3 Files** (avril 2026) : expose un bucket S3 *general purpose* en file system NFS 4.1+ pour EC2, ECS, EKS, Lambda. Basé sur la technologie EFS ; actif en cache, grosses lectures séquentielles servies depuis S3, modifications synchronisées dans le bucket. Données accessibles via file system et API S3. Sur ECS : Fargate et ECS Managed Instances, pas le launch type EC2. Pour migrations NAS, FSx reste la réponse.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Lustre scratch = le moins cher pour un traitement temporaire où la perte de données est acceptable.

## Quand l'utiliser vs alternatives

- Comparaison → [[fsx-types]] et [[ebs-vs-efs-vs-s3]]. Linux POSIX générique → [[efs]].

## Pièges d'examen

- Windows + AD = FSx for Windows.
- HPC avec intégration S3 = FSx for Lustre.
- S3 Files : pas avec le launch type EC2 d'ECS.

## Voir aussi

[[efs]], [[s3]], [[fsx-types]], [[ebs-vs-efs-vs-s3]], [[storage-gateway]], [[ecs-fargate-ecr]]
