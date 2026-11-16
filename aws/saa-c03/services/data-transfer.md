---
tags:
  - aws
  - saa-c03
  - performant
  - cost
status: to-review
source: notion
notion-module: "09. Storage extras and hybrid storage"
---

# DataSync, Transfer Family and the Snow Family

## What it is for

Move data to/from AWS: online (DataSync), over FTP/SFTP (Transfer Family) or physically (Snowball Edge).

## Key concepts

- **DataSync**: scheduled online transfer (hourly/daily/weekly) from on premises or another cloud (NFS, SMB, HDFS, S3-compatible APIs) with an **agent**; between AWS services no agent. Destinations: S3 (any class incl. Glacier), EFS, all FSx types. **Preserves permissions and metadata** (POSIX, SMB ACLs). One agent task up to 10 Gbps, bandwidth can be capped; over Direct Connect or VPN; the default recommendation for new bulk migrations.
- **Transfer Family**: managed FTP, FTPS, SFTP (and AS2) endpoints in front of S3 or EFS; Multi-AZ; users stored or via AD, LDAP, Okta, Cognito, custom IdP. Exchanging files with partners, public datasets, CRM/ERP feeds.
- **Snowball Edge**: appliances for petabytes or edge compute. Storage Optimized (~210 TB) and Compute Optimized (104 vCPUs, 416 GB RAM, 28 TB). Rule of thumb: if a network transfer takes more than a week, ship a device. 100 TB takes ~124 days at 100 Mbps, 12 days at 1 Gbps, 30 hours at 10 Gbps. A Snowball import lands in S3; Glacier through lifecycle.
- **Status (AWS docs)**: Snowcone discontinued November 2024, Snowmobile retired 2024, Snowball Edge closed to new customers since 7 November 2025 -> AWS points to DataSync (online), AWS Data Transfer Terminal (physical) and Outposts (edge). The exam guide still lists the Snow Family.

## Limits and quotas

- Snowball Edge: Storage Optimized ~210 TB; Compute Optimized 104 vCPUs / 416 GB / 28 TB.
- DataSync: up to 10 Gbps per agent task.

## Pricing (logic)

- Transfer Family: billed per provisioned endpoint per hour plus data transferred.

## When to use it vs alternatives

- Sync NFS with permissions -> DataSync. Partners over SFTP -> Transfer Family. More than a week over the network -> Snow (existing customers). Overview -> [[ebs-vs-efs-vs-s3]].

## Exam traps

- Snowball cannot import directly into Glacier.
- Synchronising NFS shares while keeping permissions = DataSync.
- Partners over SFTP = Transfer Family.

## See also

[[s3]], [[storage-gateway]], [[migration-and-tools]], [[fsx-and-s3-files]]
