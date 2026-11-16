---
tags:
  - aws
  - saa-c03
  - resilient
  - cost
status: to-review
source: notion
notion-module: "20. Disaster recovery, migration and multi-region"
---

# Server migration: the 7 Rs, MGN, Application Discovery, VMware

## What it is for

Plan and run the migration of workloads (servers, VMs, large data volumes) to AWS.

## Key concepts

- **The 7 Rs**: *Retire* (turn off), *Retain* (keep on premises), *Relocate* (no change at hypervisor level: VMware Cloud on AWS), *Rehost* (lift and shift to EC2: MGN), *Replatform* (lift, tinker and shift: database to RDS, app to Beanstalk), *Repurchase* (SaaS), *Refactor* (cloud-native: serverless, microservices).
- **Application Discovery Service**: gathers data about on-premises servers. *Agentless* (VM inventory, configuration, performance history CPU/memory/disk); *agent-based* (+ system configuration, running processes, network connections -> **dependency mapping**).
- **Application Migration Service (MGN)**: the rehost service, successor of CloudEndure Migration and Server Migration Service; continuous replication of physical, virtual and cloud servers; cutover with minimal downtime and cost.
- **VM Import/Export**: existing VMs into EC2 and back; Amazon Linux images can also run as VMs on premises (VMware, KVM, VirtualBox, Hyper-V). **VMware Cloud on AWS**: extends a vSphere data centre to AWS with VMware tools (migrate, run production across private and public clouds, DR).
- **Large data (example 200 TB on 100 Mbps)**: internet/VPN = immediate setup, ~185 days of transfer; Direct Connect 1 Gbps = ~1 month setup then ~18.5 days; Snowball (existing customers) = 2 or 3 devices in parallel, ~1 week end to end, can be combined with DMS; ongoing replication after the bulk move over VPN or DX with DMS or DataSync. New customers: DataSync over the network or Data Transfer Terminal (physical) since Snowball Edge closed in November 2025.

## Limits and quotas

- 200 TB at 100 Mbps ~ 185 days; Direct Connect 1 Gbps ~ 18.5 days after ~1 month setup.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Rehost servers -> MGN; databases -> [[dms-and-migration]]; data -> [[data-transfer]]; server DR -> [[dr-and-multi-region]].

## Exam traps

- Lift and shift of servers = Application Migration Service.
- Map dependencies = Application Discovery agent-based.

## See also

[[dms-and-migration]], [[data-transfer]], [[dr-and-multi-region]], [[elastic-beanstalk]]
