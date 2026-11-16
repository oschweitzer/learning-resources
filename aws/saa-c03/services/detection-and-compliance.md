---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "18. Security and encryption"
---

# GuardDuty, Inspector, Macie, Security Hub, Detective, Artifact

## What it is for

Detect threats, scan vulnerabilities, find sensitive data, centralise findings, investigate, get compliance reports.

## Key concepts

- **GuardDuty**: threat detection (ML, anomalies, threat intelligence), one click, no agent, 30-day trial; detects crypto-mining among others. Sources: CloudTrail (management and S3 data events), VPC Flow Logs, DNS logs; optional EKS audit logs, RDS/Aurora logins, EBS malware scans, Lambda network activity, runtime monitoring. Findings -> EventBridge.
- **Inspector**: automated vulnerability management, continuous CVE scanning, risk score: EC2 (via the SSM agent: packages and network reachability), ECR images, Lambda functions (and code). Findings -> Security Hub, EventBridge.
- **Macie**: ML and pattern matching to discover and protect sensitive data (PII) in S3; findings -> EventBridge.
- **Security Hub**: central view of findings (GuardDuty, Inspector, Macie, Firewall Manager, partners) + posture checks (CIS, AWS Foundational Security Best Practices); multi-account, uses Config.
- **Detective**: root-cause analysis of findings with a behaviour graph (CloudTrail, VPC Flow Logs, GuardDuty findings, EKS logs). **Artifact**: self-service portal for compliance reports (ISO, SOC, PCI) and agreements (BAA).

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- GuardDuty: 30-day trial.

## When to use it vs alternatives

- Threats -> GuardDuty; CVEs -> Inspector; PII in S3 -> Macie; central view -> Security Hub; investigation -> Detective.

## Exam traps

- Find PII in S3 = Macie.
- Crypto-mining = GuardDuty.
- CVE scan on ECR/Lambda/EC2 = Inspector.

## See also

[[cloudtrail]], [[vpc-flow-logs-and-network-firewall]], [[eventbridge]], [[s3]], [[config]]
