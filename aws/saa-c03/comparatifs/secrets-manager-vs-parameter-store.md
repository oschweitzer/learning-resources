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

# Secrets Manager vs Parameter Store

Built-in rotation or cheap configuration storage.

|  | Secrets Manager | Parameter Store |
| --- | --- | --- |
| Rotation | Automatic via Lambda, native for RDS/Aurora/Redshift/DocumentDB | No native rotation (advanced parameter policies: TTL/notifications) |
| Multi-region | Multi-region secrets with promotable replicas | - |
| Cost | Per secret + API calls | Standard free; advanced per parameter |
| Size | - | 4 KB standard, 8 KB advanced |

## Decision rule

- Database password rotation -> Secrets Manager.
- Cheap, hierarchical configuration -> Parameter Store.

## Exam traps

- Nothing specific in my Notion notes.

## See also

[[secrets-manager-and-parameter-store]]
