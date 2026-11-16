---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# Security groups et NACL

## À quoi ça sert

Deux pare-feux complémentaires : un niveau instance (SG) et un niveau subnet (NACL).

## Concepts clés

- Une requête traverse la **NACL** du subnet, puis le **SG** de l'instance, et la réponse revient par le même chemin.
- **NACL** : une par subnet (les nouveaux subnets reçoivent la NACL par défaut qui autorise tout in/out : ne pas la modifier, en créer de custom) ; règles numérotées de 1 à 32766, ordre croissant, **première correspondance gagne** ; dernière règle (*) refuse le reste.
- **Ports éphémères** : le client ouvre vers un port fixe et attend la réponse sur un port élevé (49152 à 65535 Windows ; 32768 à 60999 sur beaucoup de noyaux Linux). NACLs **stateless** → autoriser explicitement le retour sur 1024 à 65535, dans les deux subnets.
- **SG** : niveau instance (ENI), règles *allow* uniquement, **stateful**, toutes les règles évaluées ensemble. Peut référencer un autre SG ; un SG d'un VPC peered de la même région est référençable.
- **Bloquer une IP précise** : NACL deny (ou WAF) ; les SG ne peuvent pas refuser.
- Pattern : LB 80/443 ouverts ; instances n'acceptent que le SG du LB ; base n'accepte que le SG des instances.

## Limites et quotas

- NACL : numéros de règle 1 à 32766.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Comparatif → [[security-group-vs-nacl]]. Bloquer des IP en général → [[architectures-de-reference]].

## Pièges d'examen

- NACL stateless : penser aux ports éphémères.
- Les SG ne peuvent pas refuser.
- Bloquer une IP = NACL deny (ou WAF).

## Voir aussi

[[security-group-vs-nacl]], [[ec2]], [[vpc]], [[vpc-flow-logs-et-network-firewall]], [[waf-shield-firewall-manager]], [[architectures-de-reference]]
