---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
  - cost
statut: a-revoir
source: notion
module-notion: "03. EC2 storage"
---

# Amazon EBS : volumes et snapshots

## À quoi ça sert

Disque réseau persistant pour une instance EC2 (« clé USB réseau »).

## Concepts clés

- Un volume vit dans **une seule AZ**. Pour changer d'AZ ou de région : snapshot puis restauration.
- Attaché à une instance à la fois, sauf **Multi-Attach** (io1/io2 uniquement).
- Capacité (GiB) et performances (IOPS, débit) sont provisionnées et facturées utilisées ou non.
- *Delete on termination* : activé par défaut pour le volume root, désactivé pour les volumes additionnels.
- **Types** : gp3 (défaut, SSD), gp2 (legacy, IOPS liées à la taille), io1, io2 Block Express (SSD, base critique, latence sub-ms, durabilité 99,999 %), st1 (HDD débit, big data/logs), sc1 (HDD froid, le moins cher).
- Seuls les SSD (gp2, gp3, io1, io2) peuvent être **boot volumes**.
- **Multi-Attach** : io1/io2, jusqu'à 16 instances Nitro dans la même AZ, lecture + écriture ; appli et file system doivent être cluster-aware (pas ext4/XFS).
- **Snapshots** : incrémentaux, copiables entre AZ/régions. Archive tier (~75 % moins cher, restauration 24 à 72 h). **Recycle Bin** : conserve snapshots et AMI supprimés de 1 jour à 1 an. **Fast Snapshot Restore** : pas de pénalité de latence aux premières lectures (cher). DLM ou AWS Backup automatisent.
- **Chiffrement** (KMS, AES-256) : chiffre données au repos, en transit instance↔volume, snapshots et volumes dérivés. Activable par défaut par région.
- Chiffrer un volume existant : snapshot → copie chiffrée → nouveau volume → attacher. On peut aussi créer un volume chiffré directement depuis un snapshot non chiffré.

## Limites et quotas

- gp3 : 1 GiB à 64 TiB ; base 3 000 IOPS et 125 MiB/s ; jusqu'à 80 000 IOPS et 2 000 MiB/s, réglables indépendamment de la taille.
- gp2 : 1 GiB à 16 TiB, 3 IOPS/GiB, max 16 000 IOPS.
- io1 : 4 GiB à 16 TiB, jusqu'à 64 000 IOPS sur Nitro (32 000 sinon).
- io2 Block Express : 4 GiB à 64 TiB, jusqu'à 256 000 IOPS.
- st1 : 125 GiB à 16 TiB, jusqu'à 500 MiB/s et 500 IOPS. sc1 : jusqu'à 250 MiB/s et 250 IOPS.
- Multi-Attach : max 16 instances Nitro.

## Tarification (logique)

- Capacité et perfs facturées qu'elles soient utilisées ou non.
- Archive tier de snapshot moins cher, mais restauration lente. Fast Snapshot Restore est coûteux.

## Quand l'utiliser vs alternatives

- Comparer les types → [[ebs-volume-types]]. Partagé multi-AZ Linux → [[efs]]. Objets → [[s3]]. Local éphémère → [[ami-et-instance-store]].
- Depuis septembre 2025, io2 est surtout la réponse pour > 80 000 IOPS, latence minimale ou durabilité maximale (gp3 a monté ses limites).

## Pièges d'examen

- EBS est lié à une AZ : cross-AZ = snapshot.
- Seuls les SSD bootent.
- Pour chiffrer l'existant : snapshot copié avec chiffrement.

## Voir aussi

[[ami-et-instance-store]], [[efs]], [[ebs-vs-efs-vs-s3]], [[ebs-volume-types]], [[kms]], [[aws-backup]]
