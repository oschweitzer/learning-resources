---
tags:
  - aws
  - saa-c03
source: notion
---

# AWS Solutions Architect Associate (SAA-C03)

Notes de révision Obsidian et cartes Anki, migrées depuis mon cours Notion « AWS Solutions Architect Associate course (SAA-C03) ». Aucune information n'est ajoutée : tout vient du Notion. Les manques sont listés dans [[TODO-lacunes]].

## L'examen en bref (source : page racine Notion)

| Élément | Détail |
| --- | --- |
| Examen | AWS Certified Solutions Architect Associate, SAA-C03 (exam guide 1.1, 29 octobre 2026) |
| Format | 65 questions en 130 minutes ; 50 notées, 15 non notées (non identifiées) |
| Types | Choix unique (1 sur 4) et choix multiple (2 ou plus sur 5 ou plus) |
| Score | Échelle 100 à 1 000, réussite à 720 ; modèle compensatoire ; pas de pénalité pour les mauvaises réponses |
| Frais et nouvelle tentative | 150 USD ; nouvelle tentative possible après 14 jours |
| Expérience attendue | Au moins un an d'expérience pratique de conception sur AWS |

## Domaines, poids et notes à réviser

| Domaine | Poids | Tag | Notes principales |
| --- | --- | --- | --- |
| 1. Design secure architectures | 30 % | `secure` | [[iam]], [[iam-avance]], [[organizations-et-scp]], [[kms]], [[waf-shield-firewall-manager]], [[vpc]], [[security-groups-et-nacl]], [[s3]] |
| 2. Design resilient architectures | 26 % | `resilient` | [[elb]], [[auto-scaling]], [[rds]], [[aurora]], [[sqs]], [[sns]], [[lambda]], [[dr-et-multi-region]] |
| 3. Design high-performing architectures | 24 % | `performant` | [[ec2]], [[ebs]], [[efs]], [[s3]], [[cloudfront]], [[fsx-et-s3-files]], [[athena]] |
| 4. Design cost-optimised architectures | 20 % | `cost` | [[options-d-achat-ec2]], [[classes-de-stockage-s3]], [[gestion-des-couts]], [[vpc-endpoints]] |

Dans Obsidian, filtrer par tag : `#secure`, `#resilient`, `#performant`, `#cost`.

## Index des services

### Fondations et identité

- [[infrastructure-globale]] : Infrastructure globale AWS et responsabilité partagée
- [[iam]] : IAM : users, groups, roles, policies
- [[iam-avance]] : IAM avancé : conditions, boundaries, évaluation
- [[organizations-et-scp]] : AWS Organizations, SCP et politiques d'organisation
- [[identity-center-et-directory-service]] : IAM Identity Center et Directory Service
- [[control-tower]] : AWS Control Tower

### Calcul et scalabilité

- [[ec2]] : Amazon EC2 : instances, achat, placement
- [[auto-scaling]] : EC2 Auto Scaling
- [[elb]] : Elastic Load Balancing (ALB, NLB, GWLB, CLB)
- [[ecs-fargate-ecr]] : Amazon ECS, Fargate et ECR
- [[eks]] : Amazon EKS
- [[lambda]] : AWS Lambda
- [[step-functions]] : AWS Step Functions
- [[elastic-beanstalk]] : AWS Elastic Beanstalk
- [[services-divers]] : Autres services à connaître

### Stockage

- [[ebs]] : Amazon EBS : volumes et snapshots
- [[ami-et-instance-store]] : AMI et instance store
- [[efs]] : Amazon EFS
- [[s3]] : Amazon S3
- [[fsx-et-s3-files]] : Amazon FSx et Amazon S3 Files
- [[storage-gateway]] : AWS Storage Gateway
- [[transfert-de-donnees]] : DataSync, Transfer Family et Snow Family

### Bases de données et cache

- [[rds]] : Amazon RDS (et RDS Proxy)
- [[aurora]] : Amazon Aurora
- [[dynamodb]] : Amazon DynamoDB (et DAX)
- [[elasticache]] : Amazon ElastiCache
- [[bases-specialisees]] : Bases spécialisées : DocumentDB, Neptune, Keyspaces, Timestream, MemoryDB

### Réseau, DNS et edge

- [[vpc]] : Amazon VPC : bases, CIDR, IPv6 et coûts réseau
- [[nat]] : NAT : instance, NAT gateway zonale et régionale
- [[security-groups-et-nacl]] : Security groups et NACL
- [[vpc-endpoints]] : VPC endpoints et PrivateLink
- [[connectivite-vpc]] : Peering, Transit Gateway, VPN, Direct Connect
- [[vpc-flow-logs-et-network-firewall]] : VPC Flow Logs, Traffic Mirroring et Network Firewall
- [[route-53]] : Amazon Route 53
- [[cloudfront]] : Amazon CloudFront
- [[global-accelerator]] : AWS Global Accelerator

### Découplage et API

- [[sqs]] : Amazon SQS
- [[sns]] : Amazon SNS
- [[kinesis-et-firehose]] : Kinesis Data Streams et Amazon Data Firehose
- [[amazon-mq]] : Amazon MQ
- [[api-gateway]] : Amazon API Gateway
- [[cognito]] : Amazon Cognito

### Données et analytique

- [[athena]] : Amazon Athena
- [[redshift]] : Amazon Redshift
- [[glue-et-lake-formation]] : AWS Glue et AWS Lake Formation
- [[streaming-analytique]] : Managed Service for Apache Flink et Amazon MSK
- [[opensearch-emr-et-quick]] : OpenSearch Service, EMR et Amazon Quick
- [[services-ml]] : Services de machine learning managés

### Sécurité et chiffrement

- [[kms]] : AWS KMS et CloudHSM
- [[secrets-manager-et-parameter-store]] : Secrets Manager et SSM Parameter Store
- [[acm]] : AWS Certificate Manager (ACM)
- [[waf-shield-firewall-manager]] : AWS WAF, Shield et Firewall Manager
- [[detection-et-conformite]] : GuardDuty, Inspector, Macie, Security Hub, Detective, Artifact

### Monitoring, audit et opérations

- [[cloudwatch]] : Amazon CloudWatch (métriques, logs, alarmes)
- [[eventbridge]] : Amazon EventBridge
- [[cloudtrail]] : AWS CloudTrail
- [[config]] : AWS Config
- [[systems-manager]] : AWS Systems Manager
- [[cloudformation]] : AWS CloudFormation
- [[gestion-des-couts]] : Outils de gestion des coûts
- [[well-architected-et-trusted-advisor]] : Well-Architected Framework et Trusted Advisor

### Résilience, DR et migration

- [[dr-et-multi-region]] : Reprise après sinistre et multi-région
- [[aws-backup]] : AWS Backup
- [[dms-et-migration]] : AWS DMS, SCT et migration vers Aurora
- [[migration-et-outils]] : Migration de serveurs : 7 R, MGN, Application Discovery, VMware
- [[architectures-de-reference]] : Architectures de référence et patterns

## Comparatifs

- [[options-d-achat-ec2]] : Options d'achat EC2
- [[ebs-vs-efs-vs-s3]] : EBS vs EFS vs S3 (et autres stockages)
- [[ebs-volume-types]] : Types de volumes EBS
- [[alb-vs-nlb-vs-gwlb]] : ALB vs NLB vs GWLB (vs CLB)
- [[read-replica-vs-multi-az]] : Read replica vs Multi-AZ (RDS)
- [[cname-vs-alias]] : CNAME vs Alias (Route 53)
- [[routage-route-53]] : Les 8 politiques de routage Route 53
- [[classes-de-stockage-s3]] : Classes de stockage S3
- [[cloudfront-vs-global-accelerator]] : CloudFront vs Global Accelerator
- [[fsx-types]] : Les 4 FSx
- [[storage-gateway-types]] : Types de Storage Gateway
- [[sqs-vs-sns-vs-kinesis]] : SQS vs SNS vs Kinesis Data Streams
- [[kinesis-streams-vs-firehose]] : Kinesis Data Streams vs Amazon Data Firehose
- [[kinesis-vs-msk]] : Kinesis Data Streams vs Amazon MSK
- [[ecs-ec2-vs-fargate]] : ECS sur EC2 vs Fargate vs Managed Instances
- [[lambda-vs-batch]] : Lambda vs AWS Batch
- [[cognito-user-vs-identity-pools]] : Cognito user pools vs identity pools
- [[athena-vs-redshift]] : Athena vs Redshift
- [[cloudwatch-vs-cloudtrail-vs-config]] : CloudWatch vs CloudTrail vs Config
- [[directory-service-options]] : Options AWS Directory Service
- [[kms-vs-cloudhsm]] : KMS vs CloudHSM
- [[secrets-manager-vs-parameter-store]] : Secrets Manager vs Parameter Store
- [[waf-vs-shield]] : WAF vs Shield vs Firewall Manager
- [[nat-gateway-vs-instance]] : NAT gateway vs NAT instance (vs NAT régionale)
- [[security-group-vs-nacl]] : Security group vs NACL
- [[interface-vs-gateway-endpoint]] : Interface endpoint vs Gateway endpoint
- [[vpn-vs-direct-connect]] : Site-to-Site VPN vs Direct Connect
- [[dr-strategies]] : Stratégies de DR : backup, pilot light, warm standby, multi-site
- [[mots-cles-de-l-enonce]] : Mots-clés de l'énoncé et direction probable
- [[changements-recents-2025-2026]] : Changements récents à connaître (jusqu'en novembre 2026)

## Outils de suivi

- [[journal-erreurs]] : consigner chaque erreur de quiz.
- [[quiz-hebdo]] : modèle de quiz blanc hebdomadaire.
- [[TODO-lacunes]] : notions absentes du Notion ou sections non renseignées.
- Cartes Anki : [`anki/aws-saa-c03.tsv`](../../anki/aws-saa-c03.tsv) (voir [`anki/README.md`](../../anki/README.md)).

## Plan de révision (proposition)

Le suivi du Notion se termine par « deux examens blancs complets au-dessus de 80 % ». Ce plan est une proposition d'organisation, pas une donnée du Notion.

1. **Par domaine, dans l'ordre des poids** : sécurité (30 %), résilience (26 %), performance (24 %), coûts (20 %). Pour chaque note : lire, répondre aux cartes Anki sans regarder, passer le statut `a-revoir` → `vu`.
2. **Une fois par semaine** : un [[quiz-hebdo]] ; chaque erreur va dans le [[journal-erreurs]] avec la raison de l'erreur.
3. **Quotidien** : session Anki (réimporter le TSV ne crée pas de doublons grâce aux GUID stables).
4. **Passage à `maitrise`** : quand les cartes de la note sont acquises et qu'aucune erreur récente n'y renvoie dans le journal.
5. **Avant l'examen** : deux examens blancs complets, cible > 80 %, puis relire les comparatifs et [[mots-cles-de-l-enonce]].

## Technique d'examen (source Notion)

- Eliminer les réponses fausses, puis retenir celle qui respecte toutes les contraintes.
- Très peu de questions sont des pièges : une solution qui marche mais bien plus complexe que les autres est probablement fausse.
- Marquer les questions incertaines et y revenir ; ne jamais laisser une réponse vide.
- Lire les FAQ des services, survoler le Well-Architected Framework et le livre blanc de DR.
- Voir [[mots-cles-de-l-enonce]] et [[changements-recents-2025-2026]].

## Questions à poser au client (cadrage d'architecture, source Notion)

1. RTO et RPO attendus par application ? Vraie exigence multi-région ?
2. Contraintes réglementaires et de résidence des données (GDPR, secteur régulé) ?
3. Landing zone et organisation AWS existantes, ou départ de zéro ?
4. Comment les équipes se connectent (annuaire d'entreprise, SSO) ?
5. Faut-il relier AWS à un réseau interne (VPN, Direct Connect) ?
6. Volume d'appels vers Salesforce, et quelle édition (limites d'API) ?
7. Budget, et qui paie la facture AWS ?

Les décisions structurantes se consignent dans un **ADR** (Architecture Decision Record) : contexte, décision, alternatives écartées et pourquoi.
