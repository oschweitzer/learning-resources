---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "18. Security and encryption"
---

# Secrets Manager and SSM Parameter Store

## What it is for

Store secrets and configuration: Secrets Manager (rotation) or Parameter Store (simple config, free standard tier).

## Key concepts

- **Parameter Store**: serverless storage for config/secrets, optional KMS encryption (SecureString), versioning, IAM, EventBridge notifications, CloudFormation integration. Hierarchical paths (`/my-department/my-app/prod/db-url`), `GetParametersByPath`; can reference secrets (`/aws/reference/secretsmanager/...`) and public parameters (latest Amazon Linux AMI ID).
- **Tiers**: Standard (10,000 parameters per account/region, 4 KB value, no parameter policies, free); Advanced (100,000, 8 KB, parameter policies, paid per parameter). **Parameter policies**: TTL (Expiration), notify before expiry (ExpirationNotification), or if unchanged (NoChangeNotification), through EventBridge.
- **Secrets Manager**: built for secrets; **automatic rotation** every X days via a Lambda that generates the new secret; native integration with RDS (MySQL, PostgreSQL, SQL Server, Oracle), Aurora, Redshift, DocumentDB; KMS-encrypted; **multi-region secrets** (replicas kept in sync, promotable: multi-region apps and DR).

## Limits and quotas

- Standard: 10,000 parameters, 4 KB. Advanced: 100,000 parameters, 8 KB.

## Pricing (logic)

- Standard Parameter Store is free; Advanced is paid per parameter. Secrets Manager: per secret per month + API calls.

## When to use it vs alternatives

- Built-in rotation for databases -> Secrets Manager; cheap configuration storage -> Parameter Store. Comparison -> [[secrets-manager-vs-parameter-store]].

## Exam traps

- Automatic rotation of RDS passwords = Secrets Manager.

## See also

[[secrets-manager-vs-parameter-store]], [[kms]], [[rds]], [[lambda]], [[eventbridge]]
