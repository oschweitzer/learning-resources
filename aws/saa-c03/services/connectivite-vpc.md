---
tags:
  - aws
  - saa-c03
  - secure
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# Peering, Transit Gateway, VPN, Direct Connect

## À quoi ça sert

Relier des VPC entre eux et au réseau on-premises.

## Concepts clés

- **VPC peering** : connecte deux VPC en privé sur le réseau AWS ; CIDR sans chevauchement ; **non transitif** (A–B et B–C ≠ A–C) ; mettre à jour les route tables des deux côtés ; cross-account et cross-région ; référence de SG entre VPC peered de la même région.
- **Site-to-Site VPN** : IPsec sur internet ; **VGW** côté VPC (ASN custom possible), **CGW** côté vous (IP publique, ou celle d'un NAT device avec NAT traversal) ; activer la **route propagation** ; ICMP entrant pour pinger ; rapide, chiffré, bande passante limitée (~1,25 Gbps par tunnel vers un VGW). **CloudHub** : plusieurs sites on-premises via un VGW (hub-and-spoke, routage dynamique, via internet, faible coût). **Client VPN** : accès individuel OpenVPN au VPC.
- **Direct Connect** : connexion privée dédiée via un DX location ; VGW (ou Transit Gateway) ; *private VIF* → VPC, *public VIF* → services publics AWS (S3), sur la même connexion ; IPv4 et IPv6 ; plus de bande passante à moindre coût pour gros volumes, performance plus constante ; **DX gateway** : un DX vers des VPC de plusieurs régions. *Dedicated* 1 à 400 Gbps (port physique) ; *Hosted* 50 Mbps à 25 Gbps via partenaires (capacité modifiable). Délai souvent > 1 mois → VPN d'abord. **Non chiffré par défaut** (mais privé) → IPsec VPN dessus ou MACsec (dedicated). Résilience : *high* = 1 connexion sur 2 locations ; *maximum* = connexions séparées sur équipements séparés dans plusieurs locations ; VPN = backup moins cher.
- **Transit Gateway** : hub-and-spoke à routage **transitif** entre milliers de VPC et réseaux on-premises (VPN, DX gateway) ; régional avec peering inter-TGW ; partagé entre comptes via **RAM** ; route tables par VPC ; **seul service AWS supportant le multicast IP** ; **ECMP** sur VPN vers TGW agrège les tunnels (~2,5 Gbps par connexion VPN au lieu de 1,25 vers un VGW, et plus en ajoutant des connexions) ; un DX peut être partagé entre comptes via TGW.

## Limites et quotas

- VPN : ~1,25 Gbps par tunnel vers un VGW ; ~2,5 Gbps par connexion vers un TGW (ECMP).
- Direct Connect dedicated : 1 à 400 Gbps ; hosted : 50 Mbps à 25 Gbps.

## Tarification (logique)

- Direct Connect : meilleur coût pour gros volumes ; sortie via DX moins chère ; VPN : faible coût (CloudHub).

## Quand l'utiliser vs alternatives

- Quelques VPC → peering ; beaucoup de VPC + on-premises, transitif → Transit Gateway ; rapide et chiffré → VPN ; privé, constant, gros débit → Direct Connect. Comparatif → [[vpn-vs-direct-connect]].

## Pièges d'examen

- Peering non transitif.
- Direct Connect non chiffré par défaut et plusieurs semaines de délai.
- Beaucoup de VPC + on-premises = Transit Gateway.

## Voir aussi

[[vpn-vs-direct-connect]], [[vpc]], [[route-53]], [[vpc-endpoints]], [[dr-et-multi-region]]
