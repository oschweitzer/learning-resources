---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "18. Security and encryption"
---

# AWS Certificate Manager (ACM)

## À quoi ça sert

Provisionner, gérer et déployer des certificats TLS (publics gratuits et renouvelés automatiquement ; privés via AWS Private CA).

## Concepts clés

- S'intègre à ELB (ALB, NLB, CLB), CloudFront, API Gateway… **Pas aux instances EC2 directement** : un certificat public ACM ne s'extrait pas (certificats publics exportables : option payante).
- Demande publique : FQDN ou wildcard (`*.example.com`), validation **DNS** (préférée : un CNAME, idéale avec Route 53) ou email, attente de validation. Renouvellement auto **60 jours avant expiration**.
- Certificats **importés** (générés ailleurs) : pas de renouvellement auto ; ACM envoie des événements d'expiration quotidiens à EventBridge (dès 45 jours par défaut, configurable) ; la Config rule `acm-certificate-expiration-check` les signale.
- **Régions** : CloudFront et API Gateway edge-optimised → certificat en **us-east-1** ; API regional → région de l'API ; puis CNAME/alias Route 53.

## Limites et quotas

- Renouvellement auto 60 jours avant expiration ; événements d'expiration dès 45 jours (configurable).

## Tarification (logique)

- Certificats publics gratuits ; privés via AWS Private CA.

## Quand l'utiliser vs alternatives

- TLS sur ELB/CloudFront/API Gateway → ACM. Installer sur EC2 → impossible avec ACM public.

## Pièges d'examen

- ACM ne peut pas installer de certificats sur EC2.
- CloudFront : us-east-1.

## Voir aussi

[[elb]], [[cloudfront]], [[api-gateway]], [[route-53]], [[config]]
