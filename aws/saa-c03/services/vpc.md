---
tags:
  - aws
  - saa-c03
  - secure
  - resilient
  - cost
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# Amazon VPC : bases, CIDR, IPv6 et coûts réseau

## À quoi ça sert

Réseau privé régional : sous-réseaux publics/privés, accès internet, adressage, coûts de transfert.

## Concepts clés

- Une région regroupe des AZ. Un **VPC est régional** et couvre toutes les AZ de sa région ; un **subnet** est lié à **une seule AZ** (piège : l'AZ ne contient pas le VPC).
- **CIDR** : /32 = 1 IP, /24 = 256, /16 = 65 536, 0.0.0.0/0 = toutes ; chaque cran de masque en moins double la plage. Privées (RFC 1918) : 10.0.0.0/8, 172.16.0.0/12 (VPC par défaut en 172.31.0.0/16), 192.168.0.0/16.
- Chaque compte a un **VPC par défaut** par région (subnets publics). Pour de vraies charges : vos propres VPC. Éviter tout chevauchement de CIDR avec d'autres VPC ou le on-premises à connecter.
- **AWS réserve 5 IP par subnet** (.0 réseau, .1 routeur VPC, .2 DNS, .3 futur, .255 broadcast réservé) : besoin de 29 IP → /26 (64), pas /27 (32 − 5 = 27).
- **Internet Gateway (IGW)** : horizontalement scalable, HA, un par VPC ; inutile sans route. Subnet **public** = route 0.0.0.0/0 → IGW + IP publiques. **Bastion** en subnet public (SSH depuis le CIDR d'entreprise ; instances privées autorisent le SG du bastion) ; alternative moderne : Session Manager.
- **IPv6** : ~3,4 × 10^38 adresses, toutes publiques et routables. IPv4 ne peut pas être désactivé dans un VPC/subnet ; on ajoute IPv6 (dual-stack). Si une instance ne démarre pas en dual-stack : espace IPv4 épuisé (ajouter un CIDR IPv4). **Egress-only internet gateway** : IPv6 sortant sans entrant (équivalent NAT) ; route ::/0.
- **Coûts réseau (simplifiés, par GB)** : entrée internet gratuite ; même AZ en IP privée gratuit ; inter-AZ facturé dans chaque sens en IP privée (environ le double via IP publique/Elastic) → IP privées, même AZ si pas besoin de HA ; inter-régions facturé ; sortie internet facturée après un quota gratuit mensuel partagé sur le compte ; garder le trafic dans AWS, exécuter les requêtes là où sont les données ; Direct Connect : sortie moins chère. S3 : entrée gratuite, sortie internet facturée, Transfer Acceleration en plus, **S3 → CloudFront gratuit** et sortie CloudFront moins chère que S3 ; CRR facturée. **NAT gateway** : traitement par GB en plus du transfert ; **gateway endpoint S3/DynamoDB gratuit** → optimisation classique.
- **VPC Block Public Access** : peut bloquer le trafic internet de VPC/subnets entiers au niveau compte.

## Limites et quotas

- 5 VPC par région (soft) ; jusqu'à 5 CIDR IPv4 par VPC, chacun entre /28 (16 IP) et /16 (65 536 IP).
- 5 IP réservées par subnet.

## Tarification (logique)

- Entrée internet gratuite ; trafic inter-AZ et inter-région facturé ; NAT facturé à l'heure et au GB ; gateway endpoints gratuits.

## Quand l'utiliser vs alternatives

- Sortie privée → [[nat]] ; accès privé aux services AWS → [[vpc-endpoints]] ; pare-feu → [[security-groups-et-nacl]] ; interconnexion → [[connectivite-vpc]].

## Pièges d'examen

- Le VPC est régional, les subnets sont liés à une AZ.
- Une /27 n'offre que 27 IP utilisables.
- Gateway endpoints gratuits = optimisation de coût classique.

## Voir aussi

[[nat]], [[vpc-endpoints]], [[security-groups-et-nacl]], [[connectivite-vpc]], [[vpc-flow-logs-et-network-firewall]]
