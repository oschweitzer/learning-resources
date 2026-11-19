---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
status: to-review
source: notion
notion-module: "18. Security and encryption"
---

# KMS vs CloudHSM

Multi-tenant managed software or dedicated hardware.

|  | KMS | CloudHSM |
| --- | --- | --- |
| Tenancy | Multi-tenant | Single-tenant |
| Key control | AWS owned, AWS managed, customer managed | Customer managed only |
| Access | Multi-region keys possible, AWS API | Inside your VPC, shared with VPC peering |
| Crypto acceleration | No | SSL/TLS, Oracle TDE |
| Free tier | Yes | No |

## Decision rule

- Standard encryption integrated with services -> KMS.
- Dedicated FIPS level 3 hardware, full control -> CloudHSM.

## Exam traps

- CloudHSM can integrate with KMS through a custom key store.

## See also

[[kms]]
