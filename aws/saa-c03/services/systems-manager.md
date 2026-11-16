---
tags:
  - aws
  - saa-c03
  - secure
  - performant
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# AWS Systems Manager

## À quoi ça sert

Gérer et accéder aux instances (EC2 et on-premises) sans SSH.

## Concepts clés

- **Session Manager** : shell sécurisé sans clé SSH, bastion ni port 22 ; Linux, macOS, Windows ; logs vers S3/CloudWatch Logs ; contrôle IAM.
- **Run Command** : exécute un document/commande sur de nombreuses instances, sans SSH ; sortie vers console, S3, CloudWatch ; notifications SNS ; déclenchable par EventBridge.
- **Patch Manager** : patch OS et applis (EC2 et on-premises), à la demande ou planifié, rapports de conformité. **Maintenance Windows** : planifie des actions avec durée, instances et tâches.
- **Automation** : runbooks (documents SSM) : redémarrer des instances, créer des AMI, snapshots EBS ; manuel, EventBridge, planifié ou remédiation Config. **Parameter Store** : voir [[secrets-manager-et-parameter-store]].
- Tout nécessite l'**agent SSM** et un IAM role (instance profile) autorisant Systems Manager.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Accès sans port ouvert → Session Manager ; patching planifié → Patch Manager + Maintenance Window.

## Pièges d'examen

- Shell sans ouvrir le port 22 = Session Manager.
- Patcher 500 instances sans SSH = Patch Manager + Maintenance Window.

## Voir aussi

[[ec2]], [[config]], [[eventbridge]], [[secrets-manager-et-parameter-store]]
