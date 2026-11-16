---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "18. Security and encryption"
---

# Secrets Manager et SSM Parameter Store

## À quoi ça sert

Stocker secrets et configuration : Secrets Manager (rotation) ou Parameter Store (config simple, gratuit en standard).

## Concepts clés

- **Parameter Store** : stockage serverless de config/secrets, chiffrement KMS optionnel (SecureString), versioning, IAM, notifications EventBridge, intégration CloudFormation. Chemins hiérarchiques (`/my-department/my-app/prod/db-url`), `GetParametersByPath` ; peut référencer des secrets (`/aws/reference/secretsmanager/...`) et des paramètres publics (dernier ID d'AMI Amazon Linux).
- **Tiers** : Standard (10 000 paramètres par compte/région, valeur 4 KB, pas de parameter policies, gratuit) ; Advanced (100 000, 8 KB, parameter policies, payant par paramètre). **Parameter policies** : TTL (Expiration), notification avant expiration (ExpirationNotification), ou si inchangé (NoChangeNotification), via EventBridge.
- **Secrets Manager** : pour les secrets ; **rotation automatique** tous les X jours via une Lambda qui génère le nouveau secret ; intégration native RDS (MySQL, PostgreSQL, SQL Server, Oracle), Aurora, Redshift, DocumentDB ; chiffré KMS ; **multi-region secrets** (réplicas synchronisés, promotion possible : multi-région et DR).

## Limites et quotas

- Standard : 10 000 paramètres, 4 KB. Advanced : 100 000 paramètres, 8 KB.

## Tarification (logique)

- Parameter Store standard gratuit ; Advanced payant par paramètre. Secrets Manager : par secret et par mois + appels API.

## Quand l'utiliser vs alternatives

- Rotation intégrée pour bases → Secrets Manager ; stockage de config bon marché → Parameter Store. Comparatif → [[secrets-manager-vs-parameter-store]].

## Pièges d'examen

- Rotation automatique des mots de passe RDS = Secrets Manager.

## Voir aussi

[[secrets-manager-vs-parameter-store]], [[kms]], [[rds]], [[lambda]], [[eventbridge]]
