---
tags:
  - aws
  - saa-c03
  - secure
  - resilient
statut: a-revoir
source: notion
module-notion: "18. Security and encryption"
---

# AWS WAF, Shield et Firewall Manager

## À quoi ça sert

Protéger les applications web (WAF, couche 7), contre le DDoS (Shield) et gérer ces règles à l'échelle de l'organisation (Firewall Manager).

## Concepts clés

- **WAF** : firewall L7 sur ALB, API Gateway, CloudFront, AppSync GraphQL, Cognito user pools… **pas sur NLB** (L4). Web ACL : IP sets (jusqu'à 10 000 IP), headers, body, URI, injection SQL, XSS, taille, geo-match ; **rate-based rules** ; managed rule groups. Web ACL régionale (sauf CloudFront). IP fixe + WAF : Global Accelerator devant un ALB avec WAF.
- **Shield Standard** : gratuit et automatique, couches 3 et 4 (SYN/UDP floods, reflection). **Shield Advanced** : protège EC2, ELB, CloudFront, Global Accelerator, Route 53 contre des attaques plus sophistiquées ; équipe SRT 24/7 ; protection des coûts de scaling pendant l'attaque ; mitigation L7 automatique via règles WAF.
- **Firewall Manager** : règles de sécurité sur tous les comptes d'une organisation : WAF, Shield Advanced, security groups (EC2, ALB, ENI), Network Firewall, DNS Firewall, firewalls tiers ; policies régionales appliquées aux nouvelles ressources/comptes. WAF seul = granulaire par ressource ; Firewall Manager = automatise à l'échelle ; Shield Advanced ajoute réponse DDoS et protection des coûts.
- **Bonnes pratiques DDoS** : edge (CloudFront, Route 53, Global Accelerator) ; infrastructure (ASG, ELB, grosses instances) ; application (cache CloudFront, WAF rate-based/managed, geo restriction, règles auto Shield Advanced) ; réduction de la surface d'attaque (cacher les backends derrière CloudFront/API Gateway/ELB, SG et NACL, protéger les Elastic IP, API Gateway + WAF).

## Limites et quotas

- IP sets WAF : jusqu'à 10 000 IP.

## Tarification (logique)

- Shield Standard gratuit ; Shield Advanced : coût mensuel fixe par organisation, avec protection des coûts de scaling lors d'une attaque.

## Quand l'utiliser vs alternatives

- Injection SQL / XSS → WAF (jamais Shield). Support DDoS 24/7 + protection de coûts → Shield Advanced. Multi-comptes → Firewall Manager. Comparatif → [[waf-vs-shield]].

## Pièges d'examen

- Protection SQL injection = WAF, jamais Shield.
- WAF ne s'attache pas à un NLB.
- Bloquer une IP derrière CloudFront = WAF.

## Voir aussi

[[waf-vs-shield]], [[cloudfront]], [[elb]], [[api-gateway]], [[global-accelerator]], [[vpc-flow-logs-et-network-firewall]], [[security-groups-et-nacl]]
