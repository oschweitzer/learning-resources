---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
status: to-review
source: notion
notion-module: "03. EC2 storage"
---

# AMI and instance store

## What it is for

Boot fast and identically (AMI); get very fast but ephemeral local disks (instance store).

## Key concepts

- **AMI**: image with OS, software and configuration. **Regional**, copyable to other regions. Sources: AWS public AMIs, your own AMIs, AWS Marketplace.
- Process: customise an instance, stop it (data integrity), create the AMI (which also creates EBS snapshots), launch new instances from it.
- **Golden AMI**: shortens boot time, therefore the cooldown of an Auto Scaling group.
- **Instance store**: disks physically attached to the host, very high IOPS and throughput. **Ephemeral**: data is lost on stop, terminate or hardware failure.
- Instance store use cases: buffers, caches, scratch data, replicated data. Backups and replication are your job.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Maximum local IOPS, data can be lost -> instance store. Persistence -> [[ebs]].

## Exam traps

- Instance store loses data on stop.
- An AMI is regional (copy it to use it in another region).

## See also

[[ebs]], [[auto-scaling]], [[ec2]]
