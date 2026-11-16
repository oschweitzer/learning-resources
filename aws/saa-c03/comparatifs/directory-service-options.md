---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
status: to-review
source: notion
notion-module: "17. Advanced identity and governance"
---

# AWS Directory Service options

Which AD directory to choose.

| Option | What it is | Use it when |
| --- | --- | --- |
| AWS Managed Microsoft AD | A real AD managed by AWS, MFA; trusts possible with on-premises AD | You need AD in AWS, possibly trusted with on premises |
| AD Connector | Proxy forwarding authentication to the on-premises AD; users stay on premises | AWS apps using the existing AD without a copy |
| Simple AD | AD-compatible, Samba-based, no trust with on premises | Small, simple, standalone directory |

## Decision rule

- Existing AD without replication -> AD Connector.
- Full AD in AWS + trust -> Managed Microsoft AD.
- Small standalone directory -> Simple AD.

## Exam traps

- Identity Center integrates natively with Managed Microsoft AD.

## See also

[[identity-center-and-directory-service]]
