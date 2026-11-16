---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "03. EC2 storage"
---

# Types de volumes EBS

Choisir le type de volume selon IOPS, débit, taille et coût.

| Type | Taille | Performance | Usage |
| --- | --- | --- | --- |
| gp3 (SSD) | 1 GiB à 64 TiB | 3 000 IOPS / 125 MiB/s de base ; jusqu'à 80 000 IOPS et 2 000 MiB/s indépendants de la taille | Défaut : boot, dev/test, la plupart des bases |
| gp2 (SSD) | 1 GiB à 16 TiB | 3 IOPS/GiB, burst 3 000, max 16 000 | Legacy → migrer vers gp3 |
| io1 (SSD) | 4 GiB à 16 TiB | Jusqu'à 64 000 IOPS Nitro (32 000 sinon), 50 IOPS/GiB | Anciens volumes IOPS provisionnés |
| io2 Block Express (SSD) | 4 GiB à 64 TiB | Jusqu'à 256 000 IOPS, 1 000 IOPS/GiB, sub-ms, durabilité 99,999 % | Bases critiques, > 80 000 IOPS |
| st1 (HDD) | 125 GiB à 16 TiB | Jusqu'à 500 MiB/s et 500 IOPS | Big data, warehouses, logs (séquentiel) |
| sc1 (HDD) | 125 GiB à 16 TiB | Jusqu'à 250 MiB/s et 250 IOPS | Données froides, le moins cher |

## Règle de décision

- Par défaut → gp3.
- > 80 000 IOPS, latence minimale ou durabilité max → io2 Block Express.
- Débit séquentiel économique → st1 ; froid → sc1.
- Multi-Attach → io1/io2.

## Pièges d'examen

- Seuls les SSD bootent.
- gp3 a monté ses limites en septembre 2025.

## Voir aussi

[[ebs]]
