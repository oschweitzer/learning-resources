---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "09. Storage extras and hybrid storage"
---

# AWS Storage Gateway

## À quoi ça sert

Relier applications on-premises et stockage AWS : DR, backup/restore, stockage hiérarchisé, cache local à faible latence.

## Concepts clés

- **S3 File Gateway** : partages NFS/SMB adossés à S3 (Standard, Standard-IA, One Zone-IA, Intelligent-Tiering, puis Glacier via lifecycle), cache local des données récentes ; SMB avec auth Active Directory.
- **Volume Gateway cached** : volumes iSCSI, données récentes en cache local, le reste dans AWS (S3, snapshots EBS).
- **Volume Gateway stored** : dataset complet on-premises, backups planifiés vers S3 en snapshots EBS, restaurables on-premises ou dans EC2.
- **Tape Gateway** : bibliothèque de bandes virtuelle iSCSI sur S3 et Glacier ; garde les logiciels et processus de backup à bandes.
- La gateway tourne en VM dans votre data center ou en appliance matérielle achetée à AWS (sans plateforme de virtualisation).

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Comparaison → [[storage-gateway-types]]. Transfert bulk planifié → [[transfert-de-donnees]].

## Pièges d'examen

- Garder les bandes et logiciels de backup existants = Tape Gateway.

## Voir aussi

[[storage-gateway-types]], [[transfert-de-donnees]], [[s3]], [[fsx-et-s3-files]]
