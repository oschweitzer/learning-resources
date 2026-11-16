---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "01. Global infrastructure and IAM"
---

# IAM: users, groups, roles, policies

## What it is for

Control who can do what on which AWS resources (authentication + authorisation).

## Key concepts

- IAM is a **global** service. The **root user** is only for a handful of account-level tasks; protect it with MFA and never create access keys for it.
- A **user** maps to one physical person. A **group** contains users only (no nested groups). A user can belong to zero, one or several groups.
- A **policy** is a JSON document attached to a user, group or role. Apply least privilege.
- Structure: `Version` (always 2012-10-17), `Statement` with `Effect` (Allow/Deny), `Action`, `Resource`, optional `Condition`. `Principal` only appears in resource-based policies (e.g. S3 bucket policies).
- **Explicit Deny beats Allow**. Anything not explicitly allowed is denied by default.
- A **role** gives temporary credentials to AWS services (EC2 instance profile, Lambda execution role) or to people and other accounts through **STS**.
- Access: console = password + MFA; CLI/SDK = access keys (ID + secret), owned by one user and never shared.
- MFA options: virtual authenticator app, FIDO2 security key, passkey, hardware TOTP token. Password policy: minimum length, character types, expiry, reuse.
- Auditing: **credentials report** (account level: status of passwords, keys, MFA), **Access Advisor** (user or role: services reachable and last used, to trim policies), **Access Analyzer** (resources shared outside your zone of trust, unused access, policy validation).

## Limits and quotas

- Groups cannot be nested.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Workforce access across several accounts -> prefer [[identity-center-and-directory-service]].
- Cross-account access or access for a service -> a role. Conditions, boundaries, evaluation -> [[advanced-iam]].

## Exam traps

- An application on EC2 that needs AWS access gets an **IAM role**, never hard-coded keys.
- Explicit Deny beats any Allow.
- IAM is global, so you do not pick a region for it.

## See also

[[advanced-iam]], [[identity-center-and-directory-service]], [[organizations-and-scp]]
