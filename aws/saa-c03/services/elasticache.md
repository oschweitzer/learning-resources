---
tags:
  - aws
  - saa-c03
  - performant
status: to-review
source: notion
notion-module: "05. Relational databases and caching"
---

# Amazon ElastiCache

## What it is for

Managed in-memory cache (Valkey, Redis OSS, Memcached), sub-millisecond latency; offloads read-heavy databases and makes applications stateless (sessions).

## Key concepts

- Using a cache **requires code changes** in the application.
- **Valkey/Redis OSS**: Multi-AZ with automatic failover, read replicas, AOF persistence, backup/restore, rich types (sets, sorted sets). **Memcached**: no replication, no persistence (serverless: backup/restore), simple key-value, multi-threaded, sharding.
- Security: IAM auth for Valkey/Redis OSS + AUTH tokens + TLS; SASL for Memcached; security groups.
- **Lazy loading** (cache-aside): read the cache, on a miss read the database then write the cache; data can be stale. **Write-through**: write to the cache on every database write; no stale data but more writes. **Session store**: temporary session data with a TTL. **Leaderboards**: Valkey/Redis sorted sets.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Real-time leaderboard -> Valkey/Redis OSS (sorted sets). Cache in front of DynamoDB without code changes -> [[dynamodb]] (DAX). Sessions: ElastiCache or DynamoDB with TTL.

## Exam traps

- Lazy loading can serve stale data.
- Leaderboard = Valkey/Redis sorted sets.

## See also

[[rds]], [[dynamodb]], [[reference-architectures]]
