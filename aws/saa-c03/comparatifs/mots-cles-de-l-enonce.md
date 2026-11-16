---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
  - secure
  - cost
statut: a-revoir
source: notion
module-notion: "Page racine : Exam technique"
---

# Mots-clés de l'énoncé et direction probable

La plupart des questions sont des scénarios : éliminer les réponses fausses, puis choisir celle qui colle à toutes les contraintes. Une solution qui marche mais bien plus complexe que les autres est probablement fausse.

| Formulation dans la question | Direction probable |
| --- | --- |
| Least operational overhead | Managé ou serverless (Fargate, Lambda, DynamoDB, Aurora Serverless, services managés) |
| Most cost-effective | Spot pour l'interruptible, Savings Plans pour l'usage stable, S3 lifecycle et classes IA, gateway endpoints, right-sizing |
| Highly available | Multi-AZ, ELB + ASG, health checks Route 53 |
| Decouple, absorb spikes | SQS, SNS, EventBridge |
| Real-time streaming, replay | Kinesis Data Streams |
| Load streaming data into S3 or Redshift | Amazon Data Firehose |
| Static IP, UDP, fast regional failover | Global Accelerator ou NLB |
| SQL on files in S3 | Athena |
| Shared file system | EFS (Linux), FSx for Windows (SMB + AD), FSx for Lustre (HPC) |
| On premises to AWS, private and consistent | Direct Connect, avec un VPN en backup ou pour le chiffrement |
| Encryption keys, audit | KMS ; CloudHSM pour du matériel single-tenant |
| Who did what / Is it compliant / Is it healthy | CloudTrail / Config / CloudWatch |

## Règle de décision

- Signaler les questions hésitantes et y revenir à la fin ; ne jamais laisser une question vide.
- Lire les FAQ des services, survoler le Well-Architected Framework et le livre blanc sur le DR.

## Pièges d'examen

- Pas de pénalité pour une mauvaise réponse : toujours répondre.

## Voir aussi

[[well-architected-et-trusted-advisor]], [[cloudwatch-vs-cloudtrail-vs-config]]
