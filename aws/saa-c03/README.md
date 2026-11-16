---
tags:
  - aws
  - saa-c03
source: notion
---

# AWS Solutions Architect Associate (SAA-C03)

Obsidian revision notes and Anki cards, migrated from my Notion course "AWS Solutions Architect Associate course (SAA-C03)". Nothing is added from outside: everything comes from the Notion course. What is missing is listed in [[TODO-lacunes]].

Note statuses (frontmatter `status`): `to-review`, `seen`, `mastered`.

## The exam at a glance (source: Notion root page)

| Item | Detail |
| --- | --- |
| Exam | AWS Certified Solutions Architect Associate, SAA-C03 (exam guide 1.1, 29 October 2026) |
| Format | 65 questions in 130 minutes; 50 scored, 15 unscored and not identified |
| Question types | Multiple choice (one right answer out of four) and multiple response (two or more out of five or more) |
| Scoring | Scaled score 100 to 1,000, pass at 720; compensatory model; no penalty for guessing |
| Fee and retake | 150 USD; a failed attempt can be retaken after 14 days |
| Experience expected | At least one year of hands-on experience designing on AWS |

## Domains, weights and notes to review

| Domain | Weight | Tag | Main notes |
| --- | --- | --- | --- |
| 1. Design secure architectures | 30% | `secure` | [[iam]], [[advanced-iam]], [[organizations-and-scp]], [[kms]], [[waf-shield-firewall-manager]], [[vpc]], [[security-groups-and-nacl]], [[s3]] |
| 2. Design resilient architectures | 26% | `resilient` | [[elb]], [[auto-scaling]], [[rds]], [[aurora]], [[sqs]], [[sns]], [[lambda]], [[dr-and-multi-region]] |
| 3. Design high-performing architectures | 24% | `performant` | [[ec2]], [[ebs]], [[efs]], [[s3]], [[cloudfront]], [[fsx-and-s3-files]], [[athena]] |
| 4. Design cost-optimised architectures | 20% | `cost` | [[ec2-purchasing-options]], [[s3-storage-classes]], [[cost-management]], [[vpc-endpoints]] |

In Obsidian, filter by tag: `#secure`, `#resilient`, `#performant`, `#cost`.

## Service index

### Foundations and identity

- [[global-infrastructure]]: AWS global infrastructure and the shared responsibility model
- [[iam]]: IAM: users, groups, roles, policies
- [[advanced-iam]]: Advanced IAM: conditions, boundaries, evaluation
- [[organizations-and-scp]]: AWS Organizations, SCPs and organisation policies
- [[identity-center-and-directory-service]]: IAM Identity Center and Directory Service
- [[control-tower]]: AWS Control Tower

### Compute and scalability

- [[ec2]]: Amazon EC2: instances, purchasing, placement
- [[auto-scaling]]: EC2 Auto Scaling
- [[elb]]: Elastic Load Balancing (ALB, NLB, GWLB, CLB)
- [[ecs-fargate-ecr]]: Amazon ECS, Fargate and ECR
- [[eks]]: Amazon EKS
- [[lambda]]: AWS Lambda
- [[step-functions]]: AWS Step Functions
- [[elastic-beanstalk]]: AWS Elastic Beanstalk
- [[other-services]]: Other services worth knowing

### Storage

- [[ebs]]: Amazon EBS: volumes and snapshots
- [[ami-and-instance-store]]: AMI and instance store
- [[efs]]: Amazon EFS
- [[s3]]: Amazon S3
- [[fsx-and-s3-files]]: Amazon FSx and Amazon S3 Files
- [[storage-gateway]]: AWS Storage Gateway
- [[data-transfer]]: DataSync, Transfer Family and the Snow Family

### Databases and caching

- [[rds]]: Amazon RDS (and RDS Proxy)
- [[aurora]]: Amazon Aurora
- [[dynamodb]]: Amazon DynamoDB (and DAX)
- [[elasticache]]: Amazon ElastiCache
- [[specialized-databases]]: Specialised databases: DocumentDB, Neptune, Keyspaces, Timestream, MemoryDB

### Networking, DNS and edge

- [[vpc]]: Amazon VPC: basics, CIDR, IPv6 and network costs
- [[nat]]: NAT: instance, zonal NAT gateway and regional NAT gateway
- [[security-groups-and-nacl]]: Security groups and NACLs
- [[vpc-endpoints]]: VPC endpoints and PrivateLink
- [[vpc-connectivity]]: Peering, Transit Gateway, VPN, Direct Connect
- [[vpc-flow-logs-and-network-firewall]]: VPC Flow Logs, Traffic Mirroring and Network Firewall
- [[route-53]]: Amazon Route 53
- [[cloudfront]]: Amazon CloudFront
- [[global-accelerator]]: AWS Global Accelerator

### Decoupling and APIs

- [[sqs]]: Amazon SQS
- [[sns]]: Amazon SNS
- [[kinesis-and-firehose]]: Kinesis Data Streams and Amazon Data Firehose
- [[amazon-mq]]: Amazon MQ
- [[api-gateway]]: Amazon API Gateway
- [[cognito]]: Amazon Cognito

### Data and analytics

- [[athena]]: Amazon Athena
- [[redshift]]: Amazon Redshift
- [[glue-and-lake-formation]]: AWS Glue and AWS Lake Formation
- [[streaming-analytics]]: Managed Service for Apache Flink and Amazon MSK
- [[opensearch-emr-and-quick]]: OpenSearch Service, EMR and Amazon Quick
- [[ml-services]]: Managed machine learning services

### Security and encryption

- [[kms]]: AWS KMS and CloudHSM
- [[secrets-manager-and-parameter-store]]: Secrets Manager and SSM Parameter Store
- [[acm]]: AWS Certificate Manager (ACM)
- [[waf-shield-firewall-manager]]: AWS WAF, Shield and Firewall Manager
- [[detection-and-compliance]]: GuardDuty, Inspector, Macie, Security Hub, Detective, Artifact

### Monitoring, audit and operations

- [[cloudwatch]]: Amazon CloudWatch (metrics, logs, alarms)
- [[eventbridge]]: Amazon EventBridge
- [[cloudtrail]]: AWS CloudTrail
- [[config]]: AWS Config
- [[systems-manager]]: AWS Systems Manager
- [[cloudformation]]: AWS CloudFormation
- [[cost-management]]: Cost management tools
- [[well-architected-and-trusted-advisor]]: Well-Architected Framework and Trusted Advisor

### Resilience, DR and migration

- [[dr-and-multi-region]]: Disaster recovery and multi-region
- [[aws-backup]]: AWS Backup
- [[dms-and-migration]]: AWS DMS, SCT and migrating to Aurora
- [[migration-and-tools]]: Server migration: the 7 Rs, MGN, Application Discovery, VMware
- [[reference-architectures]]: Reference architectures and patterns

## Comparisons (`comparatifs/`)

- [[ec2-purchasing-options]]: EC2 purchasing options
- [[ebs-vs-efs-vs-s3]]: EBS vs EFS vs S3 (and other storage)
- [[ebs-volume-types]]: EBS volume types
- [[alb-vs-nlb-vs-gwlb]]: ALB vs NLB vs GWLB (vs CLB)
- [[read-replica-vs-multi-az]]: Read replica vs Multi-AZ (RDS)
- [[cname-vs-alias]]: CNAME vs Alias (Route 53)
- [[route53-routing-policies]]: The 8 Route 53 routing policies
- [[s3-storage-classes]]: S3 storage classes
- [[cloudfront-vs-global-accelerator]]: CloudFront vs Global Accelerator
- [[fsx-types]]: The 4 FSx file systems
- [[storage-gateway-types]]: Storage Gateway types
- [[sqs-vs-sns-vs-kinesis]]: SQS vs SNS vs Kinesis Data Streams
- [[kinesis-streams-vs-firehose]]: Kinesis Data Streams vs Amazon Data Firehose
- [[kinesis-vs-msk]]: Kinesis Data Streams vs Amazon MSK
- [[ecs-ec2-vs-fargate]]: ECS on EC2 vs Fargate vs Managed Instances
- [[lambda-vs-batch]]: Lambda vs AWS Batch
- [[cognito-user-vs-identity-pools]]: Cognito user pools vs identity pools
- [[athena-vs-redshift]]: Athena vs Redshift
- [[cloudwatch-vs-cloudtrail-vs-config]]: CloudWatch vs CloudTrail vs Config
- [[directory-service-options]]: AWS Directory Service options
- [[kms-vs-cloudhsm]]: KMS vs CloudHSM
- [[secrets-manager-vs-parameter-store]]: Secrets Manager vs Parameter Store
- [[waf-vs-shield]]: WAF vs Shield vs Firewall Manager
- [[nat-gateway-vs-instance]]: NAT gateway vs NAT instance (vs regional NAT)
- [[security-group-vs-nacl]]: Security group vs NACL
- [[interface-vs-gateway-endpoint]]: Interface endpoint vs Gateway endpoint
- [[vpn-vs-direct-connect]]: Site-to-Site VPN vs Direct Connect
- [[dr-strategies]]: DR strategies: backup, pilot light, warm standby, multi-site
- [[exam-keywords]]: Question keywords and the likely direction
- [[recent-changes-2025-2026]]: Recent changes to know (up to November 2026)

## Tracking tools

- [[journal-erreurs]]: log every quiz mistake.
- [[quiz-hebdo]]: weekly mock quiz template.
- [[TODO-lacunes]]: notions missing from Notion or sections left empty.
- Anki cards: [`anki/aws-saa-c03.tsv`](../../anki/aws-saa-c03.tsv) (see [`anki/README.md`](../../anki/README.md)).

## Revision plan (proposal)

The Notion revision tracker ends with "two full practice exams above 80%". This plan is a suggested organisation, not Notion data.

1. **By domain, in weight order**: security (30%), resilience (26%), performance (24%), cost (20%). For each note: read it, answer its Anki cards from memory, then move the status from `to-review` to `seen`.
2. **Once a week**: one [[quiz-hebdo]]; every mistake goes into the [[journal-erreurs]] with the reason for the error.
3. **Daily**: an Anki session (re-importing the TSV does not create duplicates thanks to stable GUIDs).
4. **Moving to `mastered`**: when the note's cards are solid and no recent journal entry points back to it.
5. **Before the exam**: two full practice exams, target > 80%, then re-read the comparisons and [[exam-keywords]].

## Exam technique (source: Notion)

- Eliminate the answers you know are wrong, then keep the one that respects every constraint.
- Very few questions are tricks: a solution that works but is far more complex than the others is probably wrong.
- Flag the questions you hesitate on and come back; never leave a question blank.
- Read the service FAQs, skim the Well-Architected Framework and the DR whitepaper.
- See [[exam-keywords]] and [[recent-changes-2025-2026]].

## Questions to ask the client (architecture scoping, source: Notion)

1. What RTO and RPO do you expect for each application? Is there a real multi-region requirement?
2. What regulatory and data residency constraints apply (GDPR, regulated sector)?
3. Do you already have a landing zone and an AWS organisation, or are we starting from scratch?
4. How do your teams sign in (corporate directory, SSO)?
5. Do we need to connect AWS to an internal network (VPN, Direct Connect)?
6. What volume of calls goes to Salesforce, and which edition do you use (for API limits)?
7. What is the budget, and who pays the AWS bill?

Every structural choice goes into an **ADR** (Architecture Decision Record): context, decision, alternatives ruled out and why they were rejected.
