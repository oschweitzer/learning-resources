---
tags:
  - aws
  - saa-c03
  - performant
status: to-review
source: notion
notion-module: "09. Storage extras and hybrid storage"
---

# Amazon FSx and Amazon S3 Files

## What it is for

Managed third-party file systems (FSx) and file access to an existing S3 bucket (S3 Files, April 2026).

## Key concepts

- **FSx for Windows File Server**: SMB/NTFS, Active Directory integration, ACLs, quotas, DFS namespaces, Multi-AZ, daily backups to S3, SSD or HDD; can also be mounted from Linux. Use cases: Windows file shares, home directories, SharePoint, lift and shift.
- **FSx for Lustre**: parallel, POSIX, hundreds of GB/s, millions of IOPS, sub-ms; reads S3 as a file system and writes results back. HPC, ML, video, finance, EDA. *Scratch*: temporary, not replicated, very high burst; *Persistent*: long term, replicated within the AZ.
- **FSx for NetApp ONTAP**: NFS, SMB, iSCSI; snapshots, replication, compression, deduplication, cloning; Linux, Windows, macOS, VMware Cloud on AWS. NetApp/NAS migrations.
- **FSx for OpenZFS**: NFS v3 to v4.2, up to 1 million IOPS sub-ms, snapshots, compression, instant cloning. ZFS migrations, dev/test clones.
- **S3 Files** (April 2026): exposes an existing general purpose S3 bucket as a shared file system (NFS 4.1+) for EC2, ECS, EKS, Lambda. Built on EFS technology; active data cached, large sequential reads served from S3, changes sync back to the bucket. Data reachable through both the file system and the S3 API. On ECS: Fargate and ECS Managed Instances, not the EC2 launch type. For NAS migrations, FSx is still the match.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Lustre scratch = cheapest for a temporary processing job where data loss is acceptable.

## When to use it vs alternatives

- Comparison -> [[fsx-types]] and [[ebs-vs-efs-vs-s3]]. Generic Linux POSIX -> [[efs]].

## Exam traps

- Windows + AD = FSx for Windows.
- HPC with S3 integration = FSx for Lustre.
- S3 Files: not with the ECS EC2 launch type.

## See also

[[efs]], [[s3]], [[fsx-types]], [[ebs-vs-efs-vs-s3]], [[storage-gateway]], [[ecs-fargate-ecr]]
