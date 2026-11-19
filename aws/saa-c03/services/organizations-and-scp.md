---
tags:
  - aws
  - saa-c03
  - secure
  - cost
status: to-review
source: notion
notion-module: "17. Advanced identity and governance"
---

# AWS Organizations, SCPs and organisation policies

## What it is for

Manage many accounts: consolidated billing, OUs, guardrail policies.

## Key concepts

- Global service; **management account** + member accounts; an account belongs to only one organisation. **Consolidated billing** (volume discounts; RI and Savings Plans shared). API to create accounts. **OUs** can be nested (by business unit, environment, project).
- **Multi-account**: an account is the strongest isolation boundary (security, quotas, blast radius, billing). CloudTrail in every account to a central bucket (log archive account); central CloudWatch Logs; cross-account administration roles; tagging standards.
- **SCP**: *maximum* permissions for users/roles in accounts of an OU/account; **grant nothing** (IAM must also allow); apply to every principal in member accounts **including root**, **never to the management account**; FullAWSAccess by default; deny list or allow list; effective permissions = intersection of SCPs from root to account; an explicit deny wins. Uses: restrict regions, prevent disabling CloudTrail, leaving the organisation, enforce tags, block a service in a sandbox OU.
- **RCP**: maximum permissions on resources (S3, KMS, SQS, Secrets Manager, STS) in member accounts, whoever the caller (data perimeter). **Tag policies** (tag keys/values, audit, prevention). **Backup policies**. AI services opt-out policies. **Centralised root access management**: remove root credentials from member accounts and run privileged root tasks from the management account.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Consolidated billing with one payment method; volume discounts, RIs and Savings Plans are shared across accounts.

## When to use it vs alternatives

- Multi-account guardrails -> SCP; turnkey landing zone -> [[control-tower]]; workforce access -> [[identity-center-and-directory-service]].

## Exam traps

- SCPs grant nothing and do not affect the management account.
- Restricting a bucket to your organisation's accounts = aws:PrincipalOrgID.

## See also

[[control-tower]], [[advanced-iam]], [[identity-center-and-directory-service]], [[cloudtrail]], [[cost-management]]
