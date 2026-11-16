---
tags:
  - aws
  - saa-c03
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# Autres services à connaître

## À quoi ça sert

Services qui apparaissent dans une ou deux questions.

## Concepts clés

- **Outposts** : racks AWS dans votre data center, mêmes APIs ; faible latence vers l'on-premises, traitement local, résidence des données ; EC2, EBS, S3, EKS, ECS, RDS, EMR ; **la sécurité physique est à vous**.
- **AWS Batch** : jobs batch managés à toute échelle ; lance EC2/Spot (ou Fargate) dynamiquement ; jobs en images Docker ; files d'attente ; multi-node parallel jobs.
- **Wavelength** : compute/stockage dans les réseaux 5G télécom. **AppFlow** : transfert sécurisé entre SaaS (Salesforce, SAP, Zendesk, Slack, ServiceNow) et AWS (S3, Redshift) ou autre SaaS, planifié/sur événement/à la demande. **Amplify** : outils et hébergement pour applis web/mobiles full-stack (auth, stockage, APIs, CI/CD, Git). **Service Catalog** : produits approuvés (templates CloudFormation) lancés en self-service. **License Manager** : suivi des licences (Microsoft, Oracle) AWS et on-premises. **Resource Access Manager (RAM)** : partage de ressources (subnets, Transit Gateways, règles Route 53 Resolver, configs License Manager) avec d'autres comptes. **Health Dashboard** : événements AWS affectant vos ressources. **Kinesis Video Streams** : vidéo d'appareils pour analytique/ML. **Data Exchange** : datasets tiers.
- **SES** : envoi/réception d'email à l'échelle ; dashboard de réputation, deliverability, feedback loops, stats livraisons/bounces/plaintes ; DKIM, SPF, IP dédiées.
- **Pinpoint** : campagnes marketing bidirectionnelles email/SMS/push/voix. **Fin de support le 30 octobre 2026**, fermé aux nouveaux clients depuis mai 2025 : API SMS/voix/push continuent sous **AWS End User Messaging**, email vers SES, fonctions d'engagement vers Amazon Connect.
- Exam guide 1.1 (octobre 2026) retire **Elastic Transcoder** des services dans le scope.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Job long dans un conteneur → Batch, pas [[lambda]] ([[lambda-vs-batch]]). Partage multi-compte → RAM.

## Pièges d'examen

- Jobs > 15 min en conteneur = Batch ou Fargate.
- Outposts : sécurité physique à votre charge.

## Voir aussi

[[lambda-vs-batch]], [[lambda]], [[ecs-fargate-ecr]], [[infrastructure-globale]], [[transfert-de-donnees]]
