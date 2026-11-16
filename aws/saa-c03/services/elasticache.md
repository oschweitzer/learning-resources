---
tags:
  - aws
  - saa-c03
  - performant
statut: a-revoir
source: notion
module-notion: "05. Relational databases and caching"
---

# Amazon ElastiCache

## À quoi ça sert

Cache en mémoire managé (Valkey, Redis OSS, Memcached), latence sub-milliseconde ; allège les bases lues intensivement et rend les applis stateless (sessions).

## Concepts clés

- Utiliser un cache **demande des changements de code** dans l'application.
- **Valkey/Redis OSS** : Multi-AZ avec failover auto, read replicas, persistance AOF, backup/restore, types riches (sets, sorted sets). **Memcached** : pas de réplication, pas de persistance (serverless : backup/restore), key-value simple, multi-thread, sharding.
- Sécurité : IAM auth pour Valkey/Redis OSS + AUTH tokens + TLS ; SASL pour Memcached ; security groups.
- **Lazy loading** (cache-aside) : lire le cache, sur miss lire la base puis écrire le cache ; données potentiellement périmées. **Write-through** : écrire dans le cache à chaque écriture en base ; pas de données périmées mais plus d'écritures. **Session store** : sessions avec TTL. **Leaderboards** : sorted sets Valkey/Redis.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Leaderboard temps réel → Valkey/Redis OSS (sorted sets). Cache devant DynamoDB sans changer le code → [[dynamodb]] (DAX). Sessions : ElastiCache ou DynamoDB avec TTL.

## Pièges d'examen

- Lazy loading = peut servir du périmé.
- Leaderboard = sorted sets Valkey/Redis.

## Voir aussi

[[rds]], [[dynamodb]], [[architectures-de-reference]]
