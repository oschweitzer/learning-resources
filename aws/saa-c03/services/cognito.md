---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "12. Serverless"
---

# Amazon Cognito

## À quoi ça sert

Authentification (user pools) et autorisation vers les ressources AWS (identity pools) pour les utilisateurs d'applis web/mobiles.

## Concepts clés

- **User pools** : *qui es-tu* ; sign-in utilisateur ; donne des **jetons JWT** ; mot de passe, reset, vérification email/téléphone, MFA, fédération (Google, Facebook, SAML, OIDC) ; s'intègre à API Gateway et ALB.
- **Identity pools** : *à quoi as-tu accès dans AWS* ; donne des **credentials AWS temporaires via STS** et un IAM role ; identités depuis user pools, logins sociaux, SAML ou accès invité ; policies pouvant utiliser l'ID utilisateur pour limiter à son préfixe S3 ou ses lignes DynamoDB ; appels directs à S3, DynamoDB, ou via API Gateway.
- Architecture mobile avec stockage par utilisateur : user pool pour le sign-in, API Gateway + Lambda + DynamoDB pour l'API, identity pool pour écrire directement dans son préfixe S3.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Authentification d'app users → user pools. Accès AWS direct → identity pools. Personnel/workforce → [[identity-center-et-directory-service]]. Comparatif → [[cognito-user-vs-identity-pools]].

## Pièges d'examen

- Les credentials AWS temporaires pour les utilisateurs d'appli viennent des identity pools, pas des user pools.

## Voir aussi

[[cognito-user-vs-identity-pools]], [[api-gateway]], [[iam]], [[identity-center-et-directory-service]]
