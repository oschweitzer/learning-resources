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

# Storage Gateway types

Which gateway for which hybrid need.

| Gateway | On-premises interface | Backed by | Use |
| --- | --- | --- | --- |
| S3 File Gateway | NFS, SMB | S3 (+ Glacier via lifecycle) | S3-backed shares with local cache |
| Volume Gateway cached | iSCSI | S3 + EBS snapshots | Recent data cached, the rest in AWS |
| Volume Gateway stored | iSCSI | Full dataset on premises, backups to S3 (EBS snapshots) | Local primary data, cloud backups |
| Tape Gateway | iSCSI VTL | S3 and Glacier | Keep tape backup software/processes |

## Decision rule

- Files -> S3 File Gateway.
- Blocks -> Volume Gateway.
- Tapes -> Tape Gateway.

## Exam traps

- Nothing specific in my Notion notes.

## See also

[[storage-gateway]]
