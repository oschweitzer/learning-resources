---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
status: to-review
source: notion
notion-module: "09. Storage extras and hybrid storage"
---

# AWS Storage Gateway

## What it is for

Bridge on-premises applications and AWS storage: DR, backup and restore, tiered storage, low-latency local cache.

## Key concepts

- **S3 File Gateway**: NFS/SMB file shares backed by S3 (Standard, Standard-IA, One Zone-IA, Intelligent-Tiering, then Glacier through lifecycle), local cache of recent data; SMB supports Active Directory auth.
- **Volume Gateway cached**: iSCSI volumes, most recent data cached locally, the rest in AWS (S3, EBS snapshots).
- **Volume Gateway stored**: full dataset on premises, scheduled backups to S3 as EBS snapshots, restorable on premises or in EC2.
- **Tape Gateway**: iSCSI virtual tape library on S3 and Glacier; keeps existing tape-based backup software and processes.
- The gateway runs as a VM in your data centre or as a hardware appliance bought from AWS when you have no virtualisation platform.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Comparison -> [[storage-gateway-types]]. Scheduled bulk transfer -> [[data-transfer]].

## Exam traps

- Keeping existing backup tapes and software = Tape Gateway.

## See also

[[storage-gateway-types]], [[data-transfer]], [[s3]], [[fsx-and-s3-files]]
