---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
statut: a-revoir
source: notion
module-notion: "18. Security and encryption"
---

# KMS vs CloudHSM

Logiciel managé multi-tenant ou matériel dédié.

|  | KMS | CloudHSM |
| --- | --- | --- |
| Tenancy | Multi-tenant | Single-tenant |
| Contrôle des clés | AWS owned, AWS managed, customer managed | Customer managed uniquement |
| Accès | Multi-region keys possibles, API AWS | Dans votre VPC, partagé via VPC peering |
| Accélération crypto | Non | SSL/TLS, Oracle TDE |
| Free tier | Oui | Non |

## Règle de décision

- Chiffrement standard intégré aux services → KMS.
- Matériel dédié FIPS niveau 3, contrôle total → CloudHSM.

## Pièges d'examen

- CloudHSM intégrable à KMS via custom key store.

## Voir aussi

[[kms]]
