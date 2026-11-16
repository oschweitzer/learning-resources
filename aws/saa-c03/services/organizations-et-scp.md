---
tags:
  - aws
  - saa-c03
  - secure
  - cost
statut: a-revoir
source: notion
module-notion: "17. Advanced identity and governance"
---

# AWS Organizations, SCP et politiques d'organisation

## À quoi ça sert

Gérer plusieurs comptes : facturation consolidée, OUs, politiques de garde-fous.

## Concepts clés

- Service global ; **management account** + comptes membres ; un compte appartient à une seule organisation. **Facturation consolidée** (remises de volume ; RI et Savings Plans partagés). API de création de comptes. **OUs** imbriquables (par business unit, environnement, projet).
- **Multi-compte** : un compte = la plus forte frontière d'isolation (sécurité, quotas, blast radius, facturation). CloudTrail dans chaque compte vers un bucket central (compte log archive) ; CloudWatch Logs central ; rôles cross-account d'administration ; standards de tags.
- **SCP** : permissions *maximales* pour users/roles des comptes d'une OU/compte ; **ne donnent rien** (IAM doit aussi autoriser) ; s'appliquent à tous les principals des comptes membres **y compris root**, **jamais au management account** ; FullAWSAccess par défaut ; deny list ou allow list ; permissions effectives = intersection des SCP de la racine au compte ; un deny explicite gagne. Usages : restreindre des régions, empêcher de désactiver CloudTrail, quitter l'organisation, imposer des tags, bloquer un service en sandbox.
- **RCP** : permissions maximales sur les ressources (S3, KMS, SQS, Secrets Manager, STS) des comptes membres, quel que soit l'appelant (data perimeter). **Tag policies** (clés/valeurs de tags, audit, prévention). **Backup policies**. Opt-out des services d'IA. **Centralised root access management** : retirer les credentials root des comptes membres et lancer des tâches root privilégiées depuis le management account.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Facturation consolidée avec un seul moyen de paiement ; les remises de volume, RI et Savings Plans sont partagés entre comptes.

## Quand l'utiliser vs alternatives

- Garde-fous multi-comptes → SCP ; landing zone clé en main → [[control-tower]] ; accès workforce → [[identity-center-et-directory-service]].

## Pièges d'examen

- SCP ne donne aucune permission et ne touche pas le management account.
- Limiter un bucket aux comptes de l'organisation = aws:PrincipalOrgID.

## Voir aussi

[[control-tower]], [[iam-avance]], [[identity-center-et-directory-service]], [[cloudtrail]], [[gestion-des-couts]]
