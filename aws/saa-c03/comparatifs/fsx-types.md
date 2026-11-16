---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
status: to-review
source: notion
notion-module: "09. Storage extras and hybrid storage"
---

# The 4 FSx file systems

Pick the right FSx file system.

| FSx | Protocols | Strengths | Use cases |
| --- | --- | --- | --- |
| Windows File Server | SMB, NTFS | AD, ACLs, quotas, DFS, Multi-AZ, backups to S3 | Windows shares, SharePoint, lift and shift |
| Lustre | Lustre (POSIX) | Hundreds of GB/s, millions of IOPS, linked to S3; scratch or persistent | HPC, ML, video, finance, EDA |
| NetApp ONTAP | NFS, SMB, iSCSI | Snapshots, replication, compression, dedup, cloning; Linux/Windows/macOS/VMware | NetApp/NAS migration |
| OpenZFS | NFS v3 to v4.2 | Up to 1M IOPS sub-ms, snapshots, instant cloning | ZFS migration, dev/test clones |

## Decision rule

- Windows + AD -> Windows File Server.
- HPC + S3 -> Lustre.
- NetApp NAS / multi-protocol -> ONTAP.
- ZFS -> OpenZFS.

## Exam traps

- Nothing specific in my Notion notes.

## See also

[[fsx-and-s3-files]]
