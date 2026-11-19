---
tags:
  - aws
  - saa-c03
  - resilient
  - secure
status: to-review
source: notion
notion-module: "20. Disaster recovery, migration and multi-region"
---

# AWS Backup

## What it is for

Centralised, managed backups across many services, no scripts.

## Key concepts

- Supports EC2, EBS, S3, RDS (all engines), Aurora, DynamoDB, DocumentDB, Neptune, EFS, FSx (Lustre, Windows, ONTAP, OpenZFS), Storage Gateway volumes, etc.
- Cross-region and cross-account copies; point-in-time recovery for supported services; on-demand and scheduled backups; tag-based policies.
- **Backup plans**: frequency (every 12 hours, daily, weekly, monthly, cron), window, transition to cold storage, retention.
- **Vault Lock**: WORM on a backup vault; nobody, not even root, can delete backups or shorten retention; a defence against ransomware and mistakes. Organizations **backup policies** deploy plans across all accounts.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Centralised multi-service backups -> AWS Backup; immutability -> Vault Lock.

## Exam traps

- Backups nobody can delete = AWS Backup Vault Lock.

## See also

[[dr-and-multi-region]], [[ebs]], [[dynamodb]], [[organizations-and-scp]], [[s3]]
