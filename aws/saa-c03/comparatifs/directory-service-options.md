---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
statut: a-revoir
source: notion
module-notion: "17. Advanced identity and governance"
---

# Options AWS Directory Service

Quel annuaire AD choisir.

| Option | Ce que c'est | À utiliser quand |
| --- | --- | --- |
| AWS Managed Microsoft AD | Vrai AD managé par AWS, MFA ; trusts possibles avec l'AD on-premises | Besoin d'AD dans AWS, éventuellement relié au on-premises |
| AD Connector | Proxy qui relaie l'authentification vers l'AD on-premises ; users restent on-premises | Applis AWS qui utilisent l'AD existant sans copie |
| Simple AD | Compatible AD, basé sur Samba, sans trust avec le on-premises | Annuaire petit, simple et autonome |

## Règle de décision

- AD existant sans réplication → AD Connector.
- AD complet dans AWS + trust → Managed Microsoft AD.
- Petit annuaire autonome → Simple AD.

## Pièges d'examen

- Identity Center s'intègre nativement à Managed Microsoft AD.

## Voir aussi

[[identity-center-et-directory-service]]
