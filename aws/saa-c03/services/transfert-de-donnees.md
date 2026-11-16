---
tags:
  - aws
  - saa-c03
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "09. Storage extras and hybrid storage"
---

# DataSync, Transfer Family et Snow Family

## À quoi ça sert

Déplacer des données vers/depuis AWS : en ligne (DataSync), via FTP/SFTP (Transfer Family) ou physiquement (Snowball Edge).

## Concepts clés

- **DataSync** : transfert en ligne planifié (horaire/quotidien/hebdo) depuis on-premises ou autre cloud (NFS, SMB, HDFS, API compatibles S3) avec **agent** ; entre services AWS sans agent. Destinations : S3 (toutes classes dont Glacier), EFS, tous les FSx. **Préserve permissions et métadonnées** (POSIX, ACL SMB). Une tâche d'agent jusqu'à 10 Gbps, bande passante plafonnable ; via Direct Connect ou VPN ; recommandation par défaut pour les nouvelles migrations bulk.
- **Transfer Family** : endpoints FTP, FTPS, SFTP (et AS2) managés devant S3 ou EFS ; Multi-AZ ; users stockés ou via AD, LDAP, Okta, Cognito, IdP custom. Échanges avec des partenaires, datasets publics, flux CRM/ERP.
- **Snowball Edge** : appliances pour des pétaoctets ou du compute en périphérie. Storage Optimized (~210 TB) et Compute Optimized (104 vCPU, 416 GB RAM, 28 TB). Règle : si le transfert réseau dure plus d'une semaine, expédier un appareil. 100 TB ≈ 124 j à 100 Mbps, 12 j à 1 Gbps, 30 h à 10 Gbps. Un import Snowball arrive dans S3 ; Glacier via lifecycle.
- **Statut (docs AWS)** : Snowcone arrêté en nov. 2024, Snowmobile retiré en 2024, Snowball Edge fermé aux nouveaux clients depuis le 7 nov. 2025 → AWS oriente vers DataSync, AWS Data Transfer Terminal (physique) et Outposts (edge). L'examen cite toujours la famille Snow.

## Limites et quotas

- Snowball Edge : Storage Optimized ~210 TB ; Compute Optimized 104 vCPU / 416 GB / 28 TB.
- DataSync : jusqu'à 10 Gbps par tâche d'agent.

## Tarification (logique)

- Transfer Family : facturé par endpoint provisionné et par heure, plus les données transférées.

## Quand l'utiliser vs alternatives

- Sync NFS avec permissions → DataSync. Partenaires en SFTP → Transfer Family. Plus d'une semaine en réseau → Snow (clients existants). Vue d'ensemble → [[ebs-vs-efs-vs-s3]].

## Pièges d'examen

- Snowball n'importe pas directement dans Glacier.
- Synchroniser des partages NFS en gardant les permissions = DataSync.
- Partenaires SFTP = Transfer Family.

## Voir aussi

[[s3]], [[storage-gateway]], [[migration-et-outils]], [[fsx-et-s3-files]]
