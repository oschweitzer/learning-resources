---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "18. Security and encryption"
---

# AWS KMS et CloudHSM

## À quoi ça sert

Gérer le chiffrement : KMS (logiciel managé par AWS) ou CloudHSM (matériel dédié, vous gérez les clés).

## Concepts clés

- Chiffrement : *in flight* (TLS/HTTPS), *at rest server-side* (clé de données gérée côté serveur), *client-side* (le serveur ne peut jamais déchiffrer ; souvent envelope encryption).
- **KMS** : IAM autorise, **CloudTrail audite chaque usage**, intégration transparente. Les KMS keys sont **régionales**. Jamais de secrets en clair dans le code.
- **Types** : *symétrique* AES-256 (utilisé par tous les services intégrés, vous ne voyez jamais la clé) ; *asymétrique* RSA/ECC (clé publique téléchargeable, utile hors AWS).
- **Propriété** : AWS owned (gratuit, invisible : SSE-S3, SSE-SQS, DynamoDB par défaut) ; AWS managed (gratuit hors appels API ; rotation auto annuelle ; aws/rds, aws/ebs, aws/s3) ; customer managed (mensuel + appels API ; rotation auto optionnelle tous les 365 j par défaut, configurable de 90 à 2 560 j, + rotation on-demand) ; imported key material (rotation manuelle uniquement).
- Rotation auto : seulement symétriques dont KMS a généré le matériel (pas asymétriques, HMAC, imported, custom key store CloudHSM). Garde le même key ID, ARN et policies.
- **Key policy** : sans key policy, personne n'utilise la clé (≠ S3). La default key policy donne au compte (root) l'accès complet, les policies IAM peuvent alors accorder l'usage. Custom key policy nécessaire pour le cross-account.
- **Déplacer des données chiffrées** : copie d'un snapshot EBS chiffré vers une autre région = re-chiffré avec une clé de la région cible. Partage d'un snapshot/AMI chiffré cross-account : partager l'objet, ajouter le compte dans la key policy, donner DescribeKey/ReEncrypt/CreateGrant/Decrypt, puis copier (idéalement re-chiffrer avec sa clé).
- **Multi-region keys** : primary + replicas avec même key ID (`mrk-`), même matériel et rotation ; chiffrer dans une région, déchiffrer dans une autre sans re-chiffrer ni appel cross-région ; pas des clés globales (policy indépendante par réplica). Usages : chiffrement client-side global, DynamoDB global tables, Aurora Global Database.
- **Réplication S3 et chiffrement** : non chiffrés et SSE-S3 répliqués par défaut ; SSE-C possible ; SSE-KMS exige l'option, une clé KMS de destination, une key policy adaptée et un IAM role avec kms:Decrypt (source) et kms:Encrypt (destination) ; attention aux quotas KMS.
- **CloudHSM** : AWS provisionne des **HSM dédiés** (single-tenant, matériel tamper-resistant FIPS niveau 3), vous gérez entièrement les clés (users, clés, opérations). Symétrique/asymétrique, signature, hachage. Clusters multi-AZ. S'intègre à KMS via un **custom key store** (EBS, S3, RDS peuvent utiliser des clés dans vos HSM). Accélération SSL/TLS, Oracle TDE.

## Limites et quotas

- Rotation auto customer managed : 365 j par défaut, configurable de 90 à 2 560 j.

## Tarification (logique)

- AWS owned gratuit ; AWS managed gratuit hors appels API ; customer managed : frais mensuels + appels API ; chaque rotation supplémentaire ajoute un coût pour les deux premières. Imported : frais mensuels.
- CloudHSM : pas de free tier.

## Quand l'utiliser vs alternatives

- Chiffrement « normal » → KMS. Matériel single-tenant, contrôle total des clés → CloudHSM. Comparatif → [[kms-vs-cloudhsm]].

## Pièges d'examen

- Les clés KMS sont régionales : copie cross-région = re-chiffrement.
- Partager une AMI/snapshot chiffré = partager aussi la clé KMS.
- Hardware single-tenant = CloudHSM.

## Voir aussi

[[kms-vs-cloudhsm]], [[secrets-manager-et-parameter-store]], [[ebs]], [[s3]], [[dynamodb]], [[iam-avance]]
