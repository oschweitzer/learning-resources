---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# Architectures de référence et patterns

## À quoi ça sert

Assembler les services : web stateless/stateful, WordPress, événementiel, cache, blocage d'IP, HPC, EC2 unique HA.

## Concepts clés

- **Stateless** (WhatIsTheTime.com) : 1 EC2 + Elastic IP (SPOF) → scale vertical puis horizontal avec Route 53 A records (les clients cachent le TTL) → ELB + ASG multi-AZ (alias, SG depuis le LB) → réserver la capacité minimale.
- **Stateful** (MyClothes.com) : sticky sessions (cassent si l'instance meurt) → panier en cookies (appli stateless mais requêtes lourdes, cookies à valider) → session ID en cookie + session dans ElastiCache/DynamoDB → données en RDS (read replicas ou ElastiCache lazy loading) → Multi-AZ RDS/ElastiCache, ELB/ASG multi-AZ, SG en chaîne.
- **WordPress** : Aurora MySQL Multi-AZ + read replicas ; images sur EBS OK pour une instance mais pas pour plusieurs → **EFS** partagé multi-AZ.
- **Lancer vite** : EC2 golden AMI + user data (ou les deux : Beanstalk) ; RDS depuis snapshot ; EBS depuis snapshot.
- **Événementiel** : SQS → Lambda (DLQ sur la file SQS, attention aux boucles de retry) ; SQS FIFO → Lambda (un message en échec bloque son groupe) ; SNS → Lambda (asynchrone, retries dans Lambda, DLQ sur la fonction) ; fan-out SNS → SQS ; S3 events (SNS/SQS/Lambda) ou S3 → EventBridge ; EventBridge + CloudTrail pour intercepter un appel d'API ; API Gateway direct vers Kinesis → Firehose → S3 sans Lambda.
- **Cache** : au edge (CloudFront ; TTL, invalidation) ; au niveau API (API Gateway caching régional) ; applicatif/base (ElastiCache, DAX) ; statique seulement via CloudFront, dynamique avec TTL court. Chaque couche échange fraîcheur contre latence, charge et coût.
- **Bloquer une IP** : EC2 public → NACL deny (SG ne peuvent pas refuser) ; ALB devant EC2 → NACL sur le subnet de l'ALB (les instances ne voient que l'ALB) ; NLB devant EC2 → NACL (IP client préservées) ; ALB avec WAF → règle IP set WAF ; CloudFront devant ALB → WAF sur CloudFront (les NACL n'aident pas, trafic des IP des edges).
- **HPC** : transfert (Direct Connect, DataSync, Snowball clients existants) ; compute (CPU/GPU, Spot Fleets + ASG, **cluster placement groups**) ; **enhanced networking (SR-IOV)** : ENA ; **EFA** = ENA amélioré pour HPC sur Linux, bypass de l'OS, MPI couplé ; stockage (io2 Block Express 256 000 IOPS, instance store, S3, EFS, FSx for Lustre) ; orchestration (Batch multi-node, ParallelCluster open source avec EFA depuis un fichier de config).
- **EC2 unique HA** : option 1 alarme CloudWatch/EventBridge → Lambda démarre une instance de secours et déplace l'Elastic IP ; option 2 ASG min=max=desired=1 sur 2 AZ, user data attache l'Elastic IP (instance role) ; pour EBS : lifecycle hook snapshot à la terminaison et hook de lancement qui recrée le volume depuis le snapshot (tagué).

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Voir les notes de chaque service cité.

## Pièges d'examen

- Bloquer une IP derrière CloudFront = WAF, pas NACL.
- Tightly coupled HPC = cluster placement group + EFA.
- DLQ d'un SNS → Lambda va sur la Lambda.

## Voir aussi

[[elb]], [[auto-scaling]], [[elasticache]], [[efs]], [[sqs]], [[sns]], [[eventbridge]], [[waf-shield-firewall-manager]], [[security-groups-et-nacl]], [[ec2]], [[lambda]]
