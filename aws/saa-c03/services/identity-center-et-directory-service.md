---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "17. Advanced identity and governance"
---

# IAM Identity Center et Directory Service

## À quoi ça sert

SSO workforce multi-comptes (Identity Center) et annuaires Active Directory dans/avec AWS (Directory Service).

## Concepts clés

- **IAM Identity Center** (ex AWS SSO) : un login pour tous les comptes de l'organisation, applis SAML 2.0 (Salesforce, Microsoft 365, Box) et instances EC2 Windows. Sources d'identité : store intégré ou externe (Active Directory, Okta, Microsoft Entra ID, OneLogin). **Permission sets** = collections de policies assignées à users/groupes par compte (rôles créés dans chaque compte). ABAC avec attributs de la source. Méthode recommandée pour la workforce multi-comptes (au lieu d'IAM users par compte).
- **Directory Service** : *AWS Managed Microsoft AD* (vrai AD managé par AWS, MFA, **trusts** avec l'AD on-premises) ; *AD Connector* (proxy qui relaie l'authentification vers l'AD on-premises, pas de copie) ; *Simple AD* (compatible AD basé sur Samba, pas de trust, petit et autonome).
- Identity Center s'intègre nativement avec AWS Managed Microsoft AD ; pour un AD auto-géré : trust bidirectionnel avec Managed AD, ou AD Connector.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Workforce multi-comptes → Identity Center ; utilisateurs d'appli → [[cognito]] ; AD existant sans réplication → AD Connector. Comparatif → [[directory-service-options]].

## Pièges d'examen

- AD on-premises sans copie dans AWS = AD Connector.

## Voir aussi

[[iam]], [[organizations-et-scp]], [[directory-service-options]], [[cognito]], [[control-tower]]
