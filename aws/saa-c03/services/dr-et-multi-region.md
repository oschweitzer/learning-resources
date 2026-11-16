---
tags:
  - aws
  - saa-c03
  - resilient
statut: a-revoir
source: notion
module-notion: "20. Disaster recovery, migration and multi-region"
---

# Reprise après sinistre et multi-région

## À quoi ça sert

Choisir une stratégie de DR selon RTO, RPO et budget ; décider entre multi-AZ et multi-région.

## Concepts clés

- **RTO** (Recovery Time Objective) = temps d'arrêt acceptable. **RPO** (Recovery Point Objective) = perte de données acceptable, en temps. Moyen mnémotechnique : RT = Time, RP = Point.
- Types de DR : on-premises → on-premises (traditionnel, très cher), on-premises → AWS (hybride), région A → région B d'AWS.
- **Stratégies, de la moins chère à la plus chère** : *Backup and restore* (heures ; coût le plus bas) ; *Pilot light* (cœur critique toujours actif, ex. base répliquée ; serveurs d'appli éteints ; dizaines de minutes) ; *Warm standby* (copie complète mais réduite, montée en charge lors de l'incident ; minutes) ; *Multi-site / hot site / active-active* (pleine échelle à deux endroits ; proche de zéro, secondes). Plus le RTO/RPO est court, plus c'est cher.
- **Astuces** : backups (snapshots EBS, RDS, S3/IA/Glacier + lifecycle, CRR ; Storage Gateway) ; haute dispo (Route 53, RDS/ElastiCache Multi-AZ, EFS, S3 ; VPN backup de DX) ; réplication (replicas cross-région, Aurora Global Database) ; automatisation (CloudFormation/Beanstalk, alarmes CloudWatch, Lambda) ; **chaos testing** ; **quotas par région → les relever dans la région de secours**.
- **Multi-AZ d'abord**, multi-région si justifié (panne régionale, latence d'utilisateurs lointains, résidence des données) ; coûte plus (transfert inter-région, infra dupliquée) et complexifie (réplication, conflits d'écriture, déploiements).
- **Router** : Route 53 (failover, latency, geolocation, weighted ; failover DNS dépend des TTL donc jamais instantané) ; Global Accelerator (2 IP anycast, bascule < 1 min) ; CloudFront (origin group).
- **Répliquer** : S3 CRR (asynchrone, RTC 15 min) ; DynamoDB global tables (multi-actif, éventuelle par défaut, last writer wins ; strong consistency option RPO zéro) ; Aurora Global Database (1 région d'écriture, 10 secondaires, lag < 1 s, promotion < 1 min) ; RDS cross-region read replica (asynchrone, promouvable) ; EBS/AMI (copies planifiées DLM/AWS Backup).
- **Checklist d'une région où l'on peut vraiment basculer** : IaC déployable à l'identique, artefacts dans les deux régions (AMI copies, ECR replication), clés et secrets répliqués (KMS multi-region keys, Secrets Manager multi-region), quotas relevés, failover testé régulièrement (un plan non testé est inexistant).
- **AWS Elastic Disaster Recovery (DRS)** (ex CloudEndure DR) : récupère serveurs physiques, virtuels et cloud dans AWS ; réplication continue au niveau bloc vers une zone de staging bon marché ; lance les instances de récupération en minutes ; supporte le failback ; RPO secondes, RTO minutes ; bases critiques, SAP, ransomware.

## Limites et quotas

- Quotas de service par région : les relever dans la région de secours.

## Tarification (logique)

- Plus le RTO/RPO est court, plus le coût augmente : backup & restore < pilot light < warm standby < multi-site.
- Multi-région : coûts de transfert inter-région et d'infra dupliquée.

## Quand l'utiliser vs alternatives

- Stratégies en détail → [[dr-strategies]]. Backups centralisés → [[aws-backup]]. DB cross-région → [[aurora]], [[dynamodb]], [[rds]].

## Pièges d'examen

- Le moins cher = backup and restore.
- Cœur toujours actif, appli éteinte = pilot light.
- Copie complète réduite = warm standby.
- RTO proche de zéro = multi-site.
- Quotas à prévoir dans la région de secours.

## Voir aussi

[[dr-strategies]], [[aws-backup]], [[aurora]], [[dynamodb]], [[route-53]], [[global-accelerator]], [[migration-et-outils]], [[kms]], [[secrets-manager-et-parameter-store]]
