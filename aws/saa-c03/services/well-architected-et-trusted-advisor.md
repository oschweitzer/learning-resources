---
tags:
  - aws
  - saa-c03
  - cost
  - resilient
  - secure
  - performant
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# Well-Architected Framework et Trusted Advisor

## À quoi ça sert

Cadre de bonnes pratiques (Well-Architected) et analyse automatique du compte (Trusted Advisor).

## Concepts clés

- **Principes** : arrêter de deviner la capacité, tester à l'échelle de la prod, automatiser, architectures évolutives, décider par la donnée, game days.
- **6 piliers** : Operational excellence, Security, Reliability, Performance efficiency, Cost optimisation, Sustainability. Ils se renforcent plutôt que de s'opposer.
- **Well-Architected Tool** : gratuit ; questions sur une charge, revue selon les 6 piliers, conseils, rapports et plans d'amélioration. Architectures de référence : AWS Architecture Center et Solutions Library.
- **Trusted Advisor** : analyse du compte sans rien installer ; recommandations en coûts, performance, sécurité, tolérance aux pannes, limites de service, excellence opérationnelle. Contrôles de base pour tous ; ensemble complet et accès via Support API avec Business ou Enterprise Support.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Well-Architected Tool gratuit. Trusted Advisor : checks de base pour tous, complet avec Business/Enterprise Support.

## Quand l'utiliser vs alternatives

- Revue d'architecture → Well-Architected Tool ; recommandations automatiques → Trusted Advisor ; right-sizing précis → [[gestion-des-couts]].

## Pièges d'examen

- 6 piliers (dont Sustainability).

## Voir aussi

[[gestion-des-couts]], [[architectures-de-reference]]
