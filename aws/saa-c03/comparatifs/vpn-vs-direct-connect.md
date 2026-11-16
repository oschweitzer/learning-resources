---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# Site-to-Site VPN vs Direct Connect

Connexion rapide et chiffrée sur internet, ou lien privé dédié.

|  | Site-to-Site VPN | Direct Connect |
| --- | --- | --- |
| Réseau | IPsec sur internet public | Liaison privée dédiée via un DX location |
| Chiffrement | Oui | Non par défaut (IPsec VPN dessus ou MACsec) |
| Délai | Rapide à mettre en place | Souvent plus d'un mois |
| Bande passante | ~1,25 Gbps par tunnel vers un VGW | Dedicated 1 à 400 Gbps ; hosted 50 Mbps à 25 Gbps |
| Perf | Variable (internet) | Plus constante, gros volumes à moindre coût |

## Règle de décision

- Besoin immédiat → VPN d'abord.
- Gros volumes, perf constante → Direct Connect, VPN en backup.

## Pièges d'examen

- DX non chiffré par défaut.

## Voir aussi

[[connectivite-vpc]]
