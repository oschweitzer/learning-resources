---
tags:
  - aws
  - saa-c03
  - performant
  - cost
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# Other services worth knowing

## What it is for

Services that show up in one or two questions.

## Key concepts

- **Outposts**: AWS racks in your data centre, same APIs; low latency to on-premises systems, local processing, data residency; EC2, EBS, S3, EKS, ECS, RDS, EMR; **you are responsible for physical security**.
- **AWS Batch**: fully managed batch jobs at any scale; launches EC2 or Spot (or Fargate) dynamically; jobs packaged as Docker images; scheduled from queues; multi-node parallel jobs.
- **Wavelength**: compute/storage inside telecom 5G networks. **AppFlow**: securely transfers data between SaaS apps (Salesforce, SAP, Zendesk, Slack, ServiceNow) and AWS (S3, Redshift) or other SaaS; on schedule, on events or on demand. **Amplify**: tools and hosting for full-stack web/mobile apps (auth, storage, APIs, CI/CD, Git). **Service Catalog**: admins publish approved products (CloudFormation templates) for self-service. **License Manager**: tracks licence usage (Microsoft, Oracle) across AWS and on premises. **Resource Access Manager (RAM)**: shares resources (subnets, Transit Gateways, Route 53 Resolver rules, License Manager configurations) with other accounts. **Health Dashboard**: AWS events affecting your resources. **Kinesis Video Streams**: device video for analytics/ML. **Data Exchange**: third-party datasets.
- **SES**: sends and receives email at scale; reputation dashboard, deliverability, feedback loops, statistics on deliveries/bounces/complaints; DKIM, SPF, dedicated IPs.
- **Pinpoint**: two-way marketing campaigns over email, SMS, push, voice. **End of support 30 October 2026**, closed to new customers since May 2025: SMS, voice and push APIs continue as **AWS End User Messaging**, email moves to SES, engagement features move to Amazon Connect.
- Exam guide 1.1 (October 2026) removes **Elastic Transcoder** from the in-scope services.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Long job in a container -> Batch, not [[lambda]] ([[lambda-vs-batch]]). Multi-account sharing -> RAM.

## Exam traps

- Jobs > 15 min in containers = Batch or Fargate.
- Outposts: physical security is yours.

## See also

[[lambda-vs-batch]], [[lambda]], [[ecs-fargate-ecr]], [[global-infrastructure]], [[data-transfer]]
