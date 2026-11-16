---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "01. Global infrastructure and IAM"
---

# IAM : users, groups, roles, policies

## À quoi ça sert

Contrôler qui peut faire quoi sur quelles ressources AWS (authentification + autorisation).

## Concepts clés

- IAM est un service **global**. Le **root user** sert à quelques tâches de niveau compte ; MFA obligatoire, jamais de clés d'accès.
- **User** = une personne physique. **Group** = contient des users uniquement (pas d'autres groupes). Un user peut être dans 0, 1 ou plusieurs groupes.
- **Policy** = document JSON attaché à un user, group ou role. Principe du moindre privilège.
- Structure : `Version` (toujours 2012-10-17), `Statement` avec `Effect` (Allow/Deny), `Action`, `Resource`, `Condition` optionnelle. `Principal` seulement dans les resource-based policies (ex. bucket policy).
- **Explicit Deny > Allow**. Ce qui n'est pas explicitement autorisé est refusé par défaut.
- **Role** = credentials temporaires pour des services AWS (instance profile EC2, execution role Lambda) ou pour des personnes / comptes via **STS**.
- Accès : console = mot de passe + MFA ; CLI/SDK = access keys (ID + secret), propres à un user, jamais partagées.
- MFA : app virtuelle, clé FIDO2, passkey, token TOTP matériel. Password policy : longueur, types de caractères, expiration, réutilisation.
- Audit : **credentials report** (compte : état des mots de passe/clés/MFA), **Access Advisor** (user ou role : services accessibles et dernière utilisation, pour réduire les policies), **Access Analyzer** (ressources partagées hors zone de confiance, accès inutilisés, validation de policy).

## Limites et quotas

- Les groupes ne peuvent pas être imbriqués.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Accès workforce multi-comptes → préférer [[identity-center-et-directory-service]].
- Accès cross-account ou pour un service → role. Conditions avancées, boundaries, évaluation → [[iam-avance]].

## Pièges d'examen

- Une appli sur EC2 qui a besoin d'AWS reçoit un **role**, jamais de clés en dur.
- Explicit Deny bat tout Allow.
- IAM est global : on ne choisit pas de région.

## Voir aussi

[[iam-avance]], [[identity-center-et-directory-service]], [[organizations-et-scp]]
