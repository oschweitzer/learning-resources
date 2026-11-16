---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "18. Security and encryption"
---

# AWS KMS and CloudHSM

## What it is for

Manage encryption: KMS (software managed by AWS) or CloudHSM (dedicated hardware, you manage the keys).

## Key concepts

- Encryption: *in flight* (TLS/HTTPS), *at rest server-side* (data key managed on the server), *client-side* (the server can never decrypt; often envelope encryption).
- **KMS**: IAM authorises, **CloudTrail audits every use**, seamless integration. KMS keys are **regional**. Never store secrets in plain text in code.
- **Types**: *symmetric* AES-256 (used by all integrated services, you never see the key); *asymmetric* RSA/ECC (public key downloadable, useful outside AWS).
- **Ownership**: AWS owned (free, invisible: SSE-S3, SSE-SQS, DynamoDB default); AWS managed (free except API calls; automatic yearly rotation; aws/rds, aws/ebs, aws/s3); customer managed (monthly + API calls; optional automatic rotation every 365 days by default, configurable 90 to 2,560 days, + on-demand rotation); imported key material (manual rotation only).
- Automatic rotation: only symmetric keys whose material KMS generated (not asymmetric, HMAC, imported, custom key store/CloudHSM). Keeps the same key ID, ARN and policies.
- **Key policy**: without a key policy nobody can use the key (unlike S3). The default key policy gives the account (root) full access so IAM policies can grant use. A custom key policy is needed for cross-account access.
- **Moving encrypted data**: copying an encrypted EBS snapshot to another region = re-encrypted with a key in the destination region. Sharing an encrypted snapshot/AMI cross-account: share the object, add the account in the key policy, grant DescribeKey/ReEncrypt/CreateGrant/Decrypt, then copy (ideally re-encrypt with its own key).
- **Multi-region keys**: a primary + replicas with the same key ID (`mrk-`), key material and rotation; encrypt in one region, decrypt in another without re-encryption or cross-region calls; not global keys (each replica has its own policy). Uses: global client-side encryption, DynamoDB global tables, Aurora Global Database.
- **S3 replication and encryption**: unencrypted and SSE-S3 objects replicate by default; SSE-C can replicate; SSE-KMS needs the option enabled, a destination KMS key, a suitable key policy and an IAM role with kms:Decrypt (source) and kms:Encrypt (destination); watch KMS quotas.
- **CloudHSM**: AWS provisions **dedicated HSMs** (single-tenant, tamper-resistant FIPS level 3 hardware); you manage the keys entirely (users, keys, operations). Symmetric/asymmetric, signing, hashing. Multi-AZ clusters. Integrates with KMS through a **custom key store** (EBS, S3, RDS can use keys in your HSMs). SSL/TLS acceleration, Oracle TDE.

## Limits and quotas

- Automatic rotation of customer managed keys: 365 days by default, configurable 90 to 2,560 days.

## Pricing (logic)

- AWS owned free; AWS managed free except API calls; customer managed: monthly fee + API calls; each additional rotation adds a cost for the first two. Imported: monthly fee.
- CloudHSM: no free tier.

## When to use it vs alternatives

- 'Normal' encryption -> KMS. Single-tenant hardware, full key control -> CloudHSM. Comparison -> [[kms-vs-cloudhsm]].

## Exam traps

- KMS keys are regional: cross-region copies are re-encrypted.
- Sharing an encrypted AMI/snapshot = also share the KMS key.
- Single-tenant hardware = CloudHSM.

## See also

[[kms-vs-cloudhsm]], [[secrets-manager-and-parameter-store]], [[ebs]], [[s3]], [[dynamodb]], [[advanced-iam]]
