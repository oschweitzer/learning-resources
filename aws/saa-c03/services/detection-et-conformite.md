---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "18. Security and encryption"
---

# GuardDuty, Inspector, Macie, Security Hub, Detective, Artifact

## À quoi ça sert

Détecter les menaces, scanner les vulnérabilités, trouver les données sensibles, centraliser les findings, enquêter, obtenir les rapports de conformité.

## Concepts clés

- **GuardDuty** : détection de menaces (ML, anomalies, threat intelligence), un clic, sans agent, essai 30 jours ; détecte entre autres le crypto-mining. Sources : CloudTrail (management et S3 data events), VPC Flow Logs, logs DNS ; options EKS audit logs, connexions RDS/Aurora, scans malware EBS, activité réseau Lambda, runtime monitoring. Findings → EventBridge.
- **Inspector** : gestion automatisée des vulnérabilités, scan continu des CVE, score de risque : EC2 (via agent SSM : packages et reachability réseau), images ECR, fonctions Lambda (et code). Findings → Security Hub, EventBridge.
- **Macie** : ML et pattern matching pour découvrir/protéger les données sensibles (PII) dans S3 ; findings → EventBridge.
- **Security Hub** : vue centrale des findings (GuardDuty, Inspector, Macie, Firewall Manager, partenaires) + contrôles de posture (CIS, AWS Foundational Security Best Practices) ; multi-compte, utilise Config.
- **Detective** : analyse de cause racine des findings via un graphe de comportement (CloudTrail, VPC Flow Logs, findings GuardDuty, logs EKS). **Artifact** : portail self-service de rapports de conformité (ISO, SOC, PCI) et accords (BAA).

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- GuardDuty : essai de 30 jours.

## Quand l'utiliser vs alternatives

- Menaces → GuardDuty ; CVE → Inspector ; PII dans S3 → Macie ; vue centrale → Security Hub ; enquête → Detective.

## Pièges d'examen

- Trouver des PII dans S3 = Macie.
- Crypto-mining = GuardDuty.
- Scan de CVE sur ECR/Lambda/EC2 = Inspector.

## Voir aussi

[[cloudtrail]], [[vpc-flow-logs-et-network-firewall]], [[eventbridge]], [[s3]], [[config]]
