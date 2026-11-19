---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# VPC Flow Logs, Traffic Mirroring and Network Firewall

## What it is for

Observe (Flow Logs, mirroring) and filter (Network Firewall) VPC traffic.

## Key concepts

- **VPC Flow Logs**: IP traffic metadata for a VPC, subnet or ENI (including managed services with ENIs: ELB, RDS, ElastiCache, Redshift, NAT gateway, Transit Gateway). Destinations: S3 (Athena), CloudWatch Logs (metric filters, alarms, Contributor Insights), Firehose. CloudWatch Logs needs an IAM role (`logs:CreateLogGroup`, `CreateLogStream`, `PutLogEvents`). Fields: srcaddr, dstaddr, srcport, dstport, protocol, packets, bytes, start, end, action (ACCEPT/REJECT), log-status.
- **Troubleshooting**: incoming request rejected -> NACL or SG; inbound ACCEPT but outbound REJECT -> NACL (SGs are stateful); outbound ACCEPT but inbound REJECT on the reply -> NACL.
- **Traffic Mirroring**: copies traffic from ENIs (all packets or filtered) to an ENI or an NLB running security appliances (content inspection, threat monitoring, troubleshooting); within a VPC or across peered VPCs.
- **Network Firewall**: protects a whole VPC from layer 3 to 7 in any direction (VPC to VPC, outbound, inbound, to/from DX and VPN); GWLB internally; centrally managed across accounts with Firewall Manager; thousands of rules: IP/port, protocol blocking (e.g. outbound SMB), stateful domain allow lists (`*.mycorp.com`), regex, TLS inspection; actions allow/drop/alert; logs to S3, CloudWatch Logs, Firehose.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Diagnose a block -> Flow Logs; domain filtering for a whole VPC -> Network Firewall; L7 filtering of a web app -> [[waf-shield-firewall-manager]].

## Exam traps

- Flow Logs: inbound ACCEPT + outbound REJECT = NACL.

## See also

[[security-groups-and-nacl]], [[cloudwatch]], [[athena]], [[waf-shield-firewall-manager]], [[detection-and-compliance]], [[vpc]]
