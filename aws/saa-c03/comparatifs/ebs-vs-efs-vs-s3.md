---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
status: to-review
source: notion
notion-module: "03, 07, 09"
---

# EBS vs EFS vs S3 (and other storage)

Which storage for which need.

| Service | Type | In one line |
| --- | --- | --- |
| S3 | Object | Durable object storage for anything |
| S3 Glacier | Object archive | Cheapest long-term archives |
| EBS | Block | Network disk for one instance (Multi-Attach aside), one AZ |
| Instance store | Block | Physical, very fast, ephemeral |
| EFS | File (NFS) | Shared POSIX for Linux, multi-AZ |
| FSx for Windows | File (SMB) | Windows file shares with AD |
| FSx for Lustre | File | HPC parallel file system linked to S3 |
| FSx for NetApp ONTAP | File and block | Broadest protocol compatibility |
| FSx for OpenZFS | File (NFS) | Managed ZFS |
| S3 Files | File on S3 | File access to an existing bucket |
| Storage Gateway | Hybrid | On-premises access to S3, EBS snapshots, virtual tapes |
| Transfer Family | Transfer | FTP/FTPS/SFTP into S3 or EFS |
| DataSync | Transfer | Scheduled online sync, keeps metadata |
| Snowball Edge | Transfer | Offline migration and edge compute (existing customers) |

## Decision rule

- Persistent disk for one instance -> EBS.
- Maximum local IOPS, loss acceptable -> instance store.
- Shared POSIX Linux multi-AZ -> EFS.
- Windows/Lustre/ONTAP/ZFS -> FSx.
- Objects, data lake, static -> S3.
- Files on data already in S3 -> S3 Files.

## Exam traps

- EBS is tied to one AZ.
- EFS is not for Windows.

## See also

[[ebs]], [[efs]], [[s3]], [[fsx-and-s3-files]], [[ami-and-instance-store]]
