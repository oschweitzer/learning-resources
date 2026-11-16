---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
  - cost
status: to-review
source: notion
notion-module: "03. EC2 storage"
---

# Amazon EBS: volumes and snapshots

## What it is for

Persistent network disk for one EC2 instance (a 'network USB stick').

## Key concepts

- A volume lives in **a single AZ**. To move it to another AZ or region: snapshot, then restore there.
- Attached to one instance at a time, except **Multi-Attach** (io1/io2 only).
- Capacity (GiB) and performance (IOPS, throughput) are provisioned and billed whether used or not.
- *Delete on termination*: on by default for the root volume, off for additional volumes.
- **Types**: gp3 (default, SSD), gp2 (legacy, IOPS tied to size), io1, io2 Block Express (SSD, critical databases, sub-ms latency, 99.999% durability), st1 (HDD throughput, big data/logs), sc1 (cold HDD, cheapest).
- Only SSD types (gp2, gp3, io1, io2) can be **boot volumes**.
- **Multi-Attach**: io1/io2, up to 16 Nitro instances in the same AZ, read and write; the application and file system must be cluster-aware (not ext4/XFS).
- **Snapshots**: incremental, copyable across AZs/regions. Archive tier (~75% cheaper, 24 to 72 h to restore). **Recycle Bin**: keeps deleted snapshots and AMIs from 1 day to 1 year. **Fast Snapshot Restore**: no latency penalty on first reads (expensive). DLM or AWS Backup automate.
- **Encryption** (KMS, AES-256): encrypts data at rest, data in transit between instance and volume, snapshots and derived volumes. Can be enabled by default per region.
- Encrypt an existing volume: snapshot -> encrypted copy -> new volume -> attach. You can also create an encrypted volume directly from an unencrypted snapshot.

## Limits and quotas

- gp3: 1 GiB to 64 TiB; baseline 3,000 IOPS and 125 MiB/s; up to 80,000 IOPS and 2,000 MiB/s, set independently of size.
- gp2: 1 GiB to 16 TiB, 3 IOPS/GiB, max 16,000 IOPS.
- io1: 4 GiB to 16 TiB, up to 64,000 IOPS on Nitro (32,000 otherwise).
- io2 Block Express: 4 GiB to 64 TiB, up to 256,000 IOPS.
- st1: 125 GiB to 16 TiB, up to 500 MiB/s and 500 IOPS. sc1: up to 250 MiB/s and 250 IOPS.
- Multi-Attach: max 16 Nitro instances.

## Pricing (logic)

- Capacity and performance are billed whether used or not.
- Snapshot archive tier is cheaper but slow to restore. Fast Snapshot Restore is expensive.

## When to use it vs alternatives

- Compare types -> [[ebs-volume-types]]. Shared multi-AZ Linux -> [[efs]]. Objects -> [[s3]]. Local ephemeral -> [[ami-and-instance-store]].
- Since September 2025, io2 is mainly the answer for > 80,000 IOPS, lowest latency or highest durability (gp3 limits were raised).

## Exam traps

- EBS is tied to one AZ: cross-AZ = snapshot.
- Only SSDs boot.
- To encrypt an existing volume: copy the snapshot with encryption.

## See also

[[ami-and-instance-store]], [[efs]], [[ebs-vs-efs-vs-s3]], [[ebs-volume-types]], [[kms]], [[aws-backup]]
