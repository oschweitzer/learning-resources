---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "17. Advanced identity and governance"
---

# Advanced IAM: conditions, boundaries, evaluation

## What it is for

Master conditions, cross-account access, permission boundaries and the policy evaluation order.

## Key concepts

- **Conditions**: `aws:SourceIp` (caller IP); `aws:RequestedRegion`; `ec2:ResourceTag/Key`, `aws:PrincipalTag/Key` (ABAC); `aws:MultiFactorAuthPresent`; `aws:PrincipalOrgID` (resource policy: only accounts of your organisation); `aws:SecureTransport` (HTTPS).
- **IAM for S3**: `s3:ListBucket` -> bucket ARN (arn:aws:s3:::bucket); `s3:GetObject`/`PutObject`/`DeleteObject` -> object ARN (arn:aws:s3:::bucket/*); a policy often needs both.
- **Cross-account**: a resource-based policy (bucket policy, SQS, SNS, Lambda, KMS key policy) names the other account -> the caller **keeps its permissions**; assuming a role -> you **give up** your original permissions for the role's. Same split for EventBridge targets.
- **Permission boundary**: a managed policy that sets the maximum permissions of an **IAM user or role (not a group)**; effective permissions = identity policy ∩ boundary ∩ SCP. Uses: let developers create their own roles without privilege escalation, delegate administration.
- **Evaluation (simplified)**: 1) an explicit deny in any policy -> deny; 2) SCPs (and RCPs) must allow; 3) resource-based policies can allow (same account); 4) permission boundary must allow; 5) session policy must allow; 6) identity-based policy must allow; 7) otherwise implicit deny.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Cap developers -> permission boundary; cap accounts -> SCP ([[organizations-and-scp]]); cross-account while keeping your rights -> resource-based policy.

## Exam traps

- Boundaries: users and roles, not groups.
- Keep your original permissions while accessing another account = resource-based policy.
- Restrict to the organisation = aws:PrincipalOrgID.

## See also

[[iam]], [[organizations-and-scp]], [[s3]], [[kms]]
