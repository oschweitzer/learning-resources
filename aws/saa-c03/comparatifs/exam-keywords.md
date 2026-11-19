---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
  - secure
  - cost
status: to-review
source: notion
notion-module: "Root page: Exam technique"
---

# Question keywords and the likely direction

Most questions are scenarios: eliminate the answers you know are wrong, then pick the one that fits every constraint. A solution that works but is far more complex than the others is probably wrong.

| Wording in the question | Likely direction |
| --- | --- |
| Least operational overhead | Managed or serverless (Fargate, Lambda, DynamoDB, Aurora Serverless, managed services) |
| Most cost-effective | Spot for interruptible work, Savings Plans for steady use, S3 lifecycle and IA classes, gateway endpoints, right-sizing |
| Highly available | Multi-AZ, ELB + ASG, Route 53 health checks |
| Decouple, absorb spikes | SQS, SNS, EventBridge |
| Real-time streaming, replay | Kinesis Data Streams |
| Load streaming data into S3 or Redshift | Amazon Data Firehose |
| Static IP, UDP, fast regional failover | Global Accelerator or NLB |
| SQL on files in S3 | Athena |
| Shared file system | EFS (Linux), FSx for Windows (SMB + AD), FSx for Lustre (HPC) |
| On premises to AWS, private and consistent | Direct Connect, with a VPN as backup or for encryption |
| Encryption keys, audit | KMS; CloudHSM for single-tenant hardware |
| Who did what / Is it compliant / Is it healthy | CloudTrail / Config / CloudWatch |

## Decision rule

- Flag questions you hesitate on and come back at the end; never leave a question blank.
- Read the service FAQs, skim the Well-Architected Framework and the disaster recovery whitepaper.

## Exam traps

- No penalty for a wrong answer: always answer.

## See also

[[well-architected-and-trusted-advisor]], [[cloudwatch-vs-cloudtrail-vs-config]]
