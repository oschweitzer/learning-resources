---
tags:
  - aws
  - saa-c03
  - resilient
status: to-review
source: notion
notion-module: "20. Disaster recovery, migration and multi-region"
---

# Disaster recovery and multi-region

## What it is for

Pick a DR strategy from RTO, RPO and budget; decide between multi-AZ and multi-region.

## Key concepts

- **RTO** (Recovery Time Objective) = acceptable downtime. **RPO** (Recovery Point Objective) = acceptable data loss, in time. Memory aid: RT = Time, RP = Point.
- Kinds of DR: on premises to on premises (traditional, very expensive), on premises to AWS (hybrid), AWS region A to region B.
- **Strategies, cheapest to most expensive**: *Backup and restore* (hours; lowest cost); *Pilot light* (critical core always running, e.g. replicated database; app servers off; tens of minutes); *Warm standby* (full but scaled-down copy, scaled up during an incident; minutes); *Multi-site / hot site / active-active* (full scale in two places; close to zero, seconds). The shorter the RTO/RPO, the higher the cost.
- **Tips**: backups (EBS snapshots, RDS, S3/IA/Glacier + lifecycle, CRR; Storage Gateway); high availability (Route 53, RDS/ElastiCache Multi-AZ, EFS, S3; VPN as backup for DX); replication (cross-region replicas, Aurora Global Database); automation (CloudFormation/Beanstalk, CloudWatch alarms, Lambda); **chaos testing**; **quotas are per region -> raise them in the standby region**.
- **Multi-AZ first**, multi-region when justified (regional outage, distant users' latency, data residency); costs more (inter-region transfer, duplicated infrastructure) and adds complexity (replication, write conflicts, deployments).
- **Routing**: Route 53 (failover, latency, geolocation, weighted; DNS failover depends on TTLs so it is never instant); Global Accelerator (2 anycast IPs, failover < 1 min); CloudFront (origin group).
- **Replicating**: S3 CRR (asynchronous, RTC 15 min); DynamoDB global tables (multi-active, eventual by default, last writer wins; strong consistency option RPO zero); Aurora Global Database (one write region, 10 secondaries, lag < 1 s, promotion < 1 min); RDS cross-region read replica (asynchronous, promotable); EBS/AMI (scheduled copies via DLM/AWS Backup).
- **Checklist for a region you can really fail over to**: all infrastructure as code deployable identically, artefacts in both regions (AMI copies, ECR replication), keys and secrets replicated (KMS multi-region keys, Secrets Manager multi-region), quotas raised, failover tested regularly (an untested DR plan should be treated as non-existent).
- **AWS Elastic Disaster Recovery (DRS)** (formerly CloudEndure DR): recovers physical, virtual and cloud servers into AWS; continuous block-level replication to a low-cost staging area; launches recovery instances in minutes; supports failback; RPO seconds, RTO minutes; critical databases, SAP, ransomware.

## Limits and quotas

- Service quotas are per region: raise them in the standby region.

## Pricing (logic)

- The shorter the RTO/RPO, the higher the cost: backup & restore < pilot light < warm standby < multi-site.
- Multi-region: inter-region transfer costs and duplicated infrastructure.

## When to use it vs alternatives

- Strategies in detail -> [[dr-strategies]]. Centralised backups -> [[aws-backup]]. Cross-region databases -> [[aurora]], [[dynamodb]], [[rds]].

## Exam traps

- Cheapest = backup and restore.
- Core always on but app off = pilot light.
- Scaled-down full copy = warm standby.
- Near-zero RTO = multi-site.
- Plan quotas in the standby region.

## See also

[[dr-strategies]], [[aws-backup]], [[aurora]], [[dynamodb]], [[route-53]], [[global-accelerator]], [[migration-and-tools]], [[kms]], [[secrets-manager-and-parameter-store]]
