---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - cost
status: to-review
source: notion
notion-module: "03. EC2 storage"
---

# EBS volume types

Pick the volume type by IOPS, throughput, size and cost.

| Type | Size | Performance | Use |
| --- | --- | --- | --- |
| gp3 (SSD) | 1 GiB to 64 TiB | 3,000 IOPS / 125 MiB/s baseline; up to 80,000 IOPS and 2,000 MiB/s independent of size | Default: boot, dev/test, most databases |
| gp2 (SSD) | 1 GiB to 16 TiB | 3 IOPS/GiB, burst 3,000, max 16,000 | Legacy -> migrate to gp3 |
| io1 (SSD) | 4 GiB to 16 TiB | Up to 64,000 IOPS on Nitro (32,000 otherwise), 50 IOPS/GiB | Older provisioned-IOPS volumes |
| io2 Block Express (SSD) | 4 GiB to 64 TiB | Up to 256,000 IOPS, 1,000 IOPS/GiB, sub-ms, 99.999% durability | Critical databases, > 80,000 IOPS |
| st1 (HDD) | 125 GiB to 16 TiB | Up to 500 MiB/s and 500 IOPS | Big data, warehouses, logs (sequential) |
| sc1 (HDD) | 125 GiB to 16 TiB | Up to 250 MiB/s and 250 IOPS | Cold data, cheapest |

## Decision rule

- Default -> gp3.
- > 80,000 IOPS, lowest latency or max durability -> io2 Block Express.
- Cheap sequential throughput -> st1; cold -> sc1.
- Multi-Attach -> io1/io2.

## Exam traps

- Only SSDs boot.
- gp3 limits were raised in September 2025.

## See also

[[ebs]]
