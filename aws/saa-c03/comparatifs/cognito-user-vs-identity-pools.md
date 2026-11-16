---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
statut: a-revoir
source: notion
module-notion: "12. Serverless"
---

# Cognito user pools vs identity pools

Authentification vs autorisation vers AWS.

|  | User pools | Identity pools |
| --- | --- | --- |
| But | Authentification (sign-in) des users de l'appli | Autorisation d'accéder aux ressources AWS |
| Donne | Jetons JWT | Credentials AWS temporaires (STS + IAM role) |
| Fonctions | Mot de passe, reset, vérification, MFA, fédération (Google, Facebook, SAML, OIDC) | Identités des user pools, logins sociaux, SAML ou invité ; policies par user ID |
| S'intègre à | API Gateway, ALB | Appels directs à S3, DynamoDB, ou via API Gateway |

## Règle de décision

- Se connecter → user pools. Accéder directement à S3/DynamoDB → identity pools.

## Pièges d'examen

- Credentials AWS temporaires = identity pools.

## Voir aussi

[[cognito]]
