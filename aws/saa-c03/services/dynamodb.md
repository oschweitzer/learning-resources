---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "12. Serverless"
---

# Amazon DynamoDB (et DAX)

## À quoi ça sert

Base NoSQL clé-valeur/document serverless, répliquée sur 3 AZ, latence à un chiffre en ms, millions de requêtes par seconde.

## Concepts clés

- Clé primaire choisie à la création : *partition key* ou partition key + *sort key*. Items jusqu'à 400 KB. Classes de table : Standard et Standard-IA (stockage moins cher pour tables rarement lues).
- **Capacité** : *Provisioned* (RCU/WCU, auto scaling optionnel ; trafic prévisible, maîtrise du coût) ; *On-demand* (scale instantané, paiement par requête, plus cher par requête ; trafic imprévisible, nouvelles applis).
- **DAX** : cache en mémoire pour DynamoDB, lectures en microsecondes, sans changer la logique applicative ; TTL par défaut 5 min ; cache items et résultats de requête (ElastiCache pour des résultats calculés/agrégés).
- **DynamoDB Streams** : flux ordonné des changements au niveau item, conservé 24 h, lu par Lambda ou KCL. **Kinesis Data Streams for DynamoDB** : jusqu'à 1 an, plus de consommateurs. Usages : emails de bienvenue, analytique temps réel, tables dérivées, indexation OpenSearch.
- **Global tables** : réplication multi-région active-active (lectures/écritures partout), Streams requis. Défaut : cohérence éventuelle multi-région (1 à 2 s en général). **Multi-region strong consistency** (GA juin 2025, choisie à la création) : **RPO zéro**.
- **TTL** : suppression auto après un timestamp (sessions, rétention réglementaire). DynamoDB peut remplacer ElastiCache comme session store grâce au TTL.
- **PITR** : restauration à la seconde sur jusqu'à 35 jours, dans une nouvelle table. Sauvegardes à la demande jusqu'à suppression (gérables par AWS Backup, copies cross-région).
- **Export vers S3** (PITR requis, sans consommer de lecture) pour Athena/audits ; **import depuis S3** (CSV, DynamoDB JSON, Ion) crée une nouvelle table sans consommer d'écriture.

## Limites et quotas

- Item jusqu'à 400 KB.
- Streams : rétention 24 h.
- PITR : jusqu'à 35 jours.
- DAX : TTL par défaut 5 min.

## Tarification (logique)

- Provisioned : capacité en RCU/WCU ; on-demand : paiement par requête (plus cher par requête). Standard-IA pour stockage moins cher.
- Export S3 et import S3 ne consomment pas de capacité.

## Quand l'utiliser vs alternatives

- Cache microsecondes → DAX. Recherche plein texte → DynamoDB Streams → Lambda → [[opensearch-emr-et-quick]]. Relationnel/joins → [[rds]] / [[aurora]]. Sessions → ElastiCache ou DynamoDB + TTL ([[elasticache]]).

## Pièges d'examen

- Pics imprévisibles = on-demand.
- Microsecondes sans changer le code = DAX.
- Zéro perte de données multi-région = global tables avec strong consistency.

## Voir aussi

[[elasticache]], [[lambda]], [[kms]], [[aws-backup]], [[bases-specialisees]], [[api-gateway]], [[dr-et-multi-region]], [[opensearch-emr-et-quick]]
