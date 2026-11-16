---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "16. Monitoring and audit"
---

# Amazon CloudWatch (métriques, logs, alarmes)

## À quoi ça sert

Observer la performance, agréger les logs, alerter. Répond à « comment ça se comporte ? ».

## Concepts clés

- **Metrics** : chaque service publie ses métriques ; namespaces, dimensions (30 max), timestamps ; custom metrics en résolution standard (1 min) ou haute (jusqu'à 1 s). **EC2 : RAM et disque absents par défaut** → CloudWatch agent. Dashboards cross-région/compte. **Metric Streams** : quasi temps réel vers Firehose ou tiers (Datadog, Dynatrace, New Relic, Splunk).
- **Logs** : log groups (souvent un par appli) → log streams. Rétention 1 jour à 10 ans ou illimitée ; chiffrés par défaut. Sources : SDK, agent, Beanstalk, ECS, Lambda, VPC Flow Logs, API Gateway, CloudTrail, requêtes Route 53. **Logs Insights** = langage de requête (pas temps réel). **Export S3** (CreateExportTask) jusqu'à 12 h : pas temps réel. **Subscription filters** : temps réel vers Kinesis Data Streams, Firehose, Lambda ; cross-account via destination + access policy (centraliser les logs). **Metric filters** : patterns de logs → métriques → alarmes.
- Depuis EC2/serveurs : rien par défaut → installer le **unified agent** (logs + métriques système : RAM, swap, disque, processus, netstat) avec permissions IAM ; l'ancien Logs agent est legacy.
- **Alarms** : états OK, INSUFFICIENT_DATA, ALARM ; période 10 ou 30 s (haute résolution) ou multiples de 60. Cibles : actions EC2 (stop, terminate, reboot, recover), Auto Scaling, SNS, Systems Manager. **Composite alarms** (AND/OR) réduisent le bruit. **EC2 recovery** : sur échec du system status check, déplace l'instance sur un autre matériel en gardant IP privée/publique/Elastic, metadata, placement group. Tester : `aws cloudwatch set-alarm-state`.
- **Autres** : Container Insights (ECS, EKS, Kubernetes), Lambda Insights, **Contributor Insights** (top-N, ex. IP qui génèrent le plus de trafic dans les Flow Logs), Application Insights, Network Synthetic Monitor (sondes ICMP/TCP VPC → on-premises).
- Hors CloudWatch : X-Ray (traces de requêtes distribuées), Health Dashboard (événements AWS affectant vos ressources), Managed Grafana et Managed Service for Prometheus.

## Limites et quotas

- Dimensions : 30 max par métrique.
- Rétention des logs : 1 jour à 10 ans, ou illimitée.
- Export S3 jusqu'à 12 h.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Perf/logs/alertes → CloudWatch ; qui a fait quoi → [[cloudtrail]] ; conformité de config → [[config]]. Comparatif → [[cloudwatch-vs-cloudtrail-vs-config]].

## Pièges d'examen

- RAM sur EC2 = CloudWatch agent.
- Logs temps réel = subscription filters, pas export S3.

## Voir aussi

[[cloudtrail]], [[config]], [[eventbridge]], [[cloudwatch-vs-cloudtrail-vs-config]], [[vpc-flow-logs-et-network-firewall]], [[auto-scaling]]
