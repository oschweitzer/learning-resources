---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "17. Advanced identity and governance"
---

# AWS Control Tower

## À quoi ça sert

Mettre en place et gouverner un environnement multi-comptes sécurisé et conforme (landing zone) en quelques clics, sur Organizations.

## Concepts clés

- Crée une **Security OU** avec un compte *Log Archive* (logs centraux) et un compte *Audit* (accès de l'équipe sécurité) ; configure en général IAM Identity Center ; **Account Factory** pour créer de nouveaux comptes conformes.
- **Controls (guardrails)** : *préventifs* (SCP, ex. restreindre les régions), *détectifs* (Config rules, ex. ressources sans tag, buckets publics), *proactifs* (CloudFormation hooks, bloquent une ressource non conforme avant déploiement).
- Dashboard de conformité multi-comptes ; détection et remédiation de la dérive (drift).

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Landing zone + gouvernance continue → Control Tower ; garde-fous manuels → [[organizations-et-scp]].

## Pièges d'examen

- Détectif = Config ; préventif = SCP ; proactif = CloudFormation hooks.

## Voir aussi

[[organizations-et-scp]], [[config]], [[identity-center-et-directory-service]], [[cloudformation]]
