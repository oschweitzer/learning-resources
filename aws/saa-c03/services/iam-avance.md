---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "17. Advanced identity and governance"
---

# IAM avancé : conditions, boundaries, évaluation

## À quoi ça sert

Maîtriser les conditions, le cross-account, les permission boundaries et l'ordre d'évaluation des policies.

## Concepts clés

- **Conditions** : `aws:SourceIp` (IP appelante) ; `aws:RequestedRegion` ; `ec2:ResourceTag/Key`, `aws:PrincipalTag/Key` (ABAC) ; `aws:MultiFactorAuthPresent` ; `aws:PrincipalOrgID` (resource policy : seulement les comptes de l'organisation) ; `aws:SecureTransport` (HTTPS).
- **IAM pour S3** : `s3:ListBucket` → ARN du bucket (arn:aws:s3:::bucket) ; `s3:GetObject`/`PutObject`/`DeleteObject` → ARN des objets (arn:aws:s3:::bucket/*) ; souvent les deux.
- **Cross-account** : resource-based policy (bucket policy, SQS, SNS, Lambda, clé KMS) nomme l'autre compte → l'appelant **garde ses permissions** ; assumer un rôle → on **abandonne** ses permissions d'origine pour celles du rôle. Même clivage pour les cibles EventBridge.
- **Permission boundary** : managed policy qui fixe le maximum d'un **user ou role (pas un groupe)** ; permissions effectives = identity policy ∩ boundary ∩ SCP. Usages : laisser les devs créer leurs rôles sans escalade de privilèges, déléguer l'administration.
- **Évaluation (simplifiée)** : 1) un deny explicite partout → deny ; 2) SCP (et RCP) doivent autoriser ; 3) resource-based policies peuvent autoriser (même compte) ; 4) permission boundary doit autoriser ; 5) session policy doit autoriser ; 6) identity-based policy doit autoriser ; 7) sinon deny implicite.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Plafonner des devs → permission boundary ; plafonner des comptes → SCP ([[organizations-et-scp]]) ; cross-account en gardant ses droits → resource-based policy.

## Pièges d'examen

- Boundaries : users et roles, pas groupes.
- Garder ses permissions en accédant à un autre compte = resource-based policy.
- Restreindre à l'organisation = aws:PrincipalOrgID.

## Voir aussi

[[iam]], [[organizations-et-scp]], [[s3]], [[kms]]
