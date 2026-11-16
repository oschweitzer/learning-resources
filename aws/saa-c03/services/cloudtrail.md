---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "16. Monitoring and audit"
---

# AWS CloudTrail

## À quoi ça sert

Audit et gouvernance : enregistre les appels d'API du compte (console, SDK, CLI, services AWS). Répond à « qui a fait quoi, quand, d'où ? ».

## Concepts clés

- **Activé par défaut**. *Event history* conserve les management events **90 jours** ; un **trail** envoie vers S3 (et CloudWatch Logs) pour la rétention longue, puis Athena. Trail toutes régions (recommandé) ou une seule ; **organization trail** pour tous les comptes.
- **Types d'événements** : *management* (IAM AttachRolePolicy, EC2 CreateSubnet… ; logués par défaut ; read/write) ; *data events* (S3 GetObject/PutObject/DeleteObject, Lambda Invoke ; **non** par défaut, volume élevé) ; *Insights* (activité inhabituelle ; non par défaut, payant ; baseline sur les write management events, vers console, S3, EventBridge).
- **CloudTrail Lake** : stockage et requêtes SQL sur longue durée.
- Question typique : quelqu'un a supprimé une ressource, qui ? → CloudTrail.

## Limites et quotas

- Event history : 90 jours.

## Tarification (logique)

- Data events et Insights payants/optionnels (volume élevé).

## Quand l'utiliser vs alternatives

- Qui a fait quoi → CloudTrail ; performance → [[cloudwatch]] ; conformité → [[config]]. Comparatif → [[cloudwatch-vs-cloudtrail-vs-config]].

## Pièges d'examen

- CloudTrail ne garde que 90 jours sans trail.
- Appels S3 objet = data events, à activer.
- Qui a supprimé = CloudTrail.

## Voir aussi

[[cloudwatch]], [[config]], [[eventbridge]], [[athena]], [[cloudwatch-vs-cloudtrail-vs-config]], [[organizations-et-scp]]
