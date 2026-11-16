---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
  - cost
statut: a-revoir
source: notion
module-notion: "12. Serverless"
---

# AWS Lambda

## À quoi ça sert

Fonctions à la demande, scaling automatique, intégrées à la plupart des services AWS (monitoring CloudWatch).

## Concepts clés

- EC2 = serveur virtuel limité par RAM/CPU, en continu. Lambda = fonction limitée par la durée, à la demande, scale auto.
- Runtimes : Node.js, Python, Java, .NET, Ruby, runtimes custom (Rust, Go), images de conteneur implémentant la Lambda Runtime API. Docker arbitraire → ECS/Fargate.
- **Invocation** : *synchrone* (API Gateway, ALB, SDK ; si throttlé → 429, l'appelant réessaie) ; *asynchrone* (S3, SNS, EventBridge ; queue + retries jusqu'à 6 h, échecs vers DLQ ou on-failure destination) ; *event source mapping* (SQS, Kinesis, DynamoDB Streams : Lambda poll par lots).
- **Reserved concurrency** : garantit et plafonne une fonction (sans elle, une fonction peut throttler toutes les autres). **Provisioned concurrency** : environnements pré-initialisés, plus de cold start, payant, scalable via Application Auto Scaling. **SnapStart** (Java, Python, .NET) : snapshot après init, jusqu'à 10x plus rapide, sans coût.
- **Cold start** : nouvel environnement charge le code et exécute l'init hors handler.
- **Dans un VPC** : par défaut Lambda tourne dans un VPC AWS (internet oui, ressources privées non). Attachée à votre VPC (subnets + SGs) → ENI ; accès internet via **NAT gateway** ; services AWS via VPC endpoints (DynamoDB : gateway endpoint gratuit). Beaucoup de connexions DB → **RDS Proxy** (la fonction doit être dans le VPC).
- RDS PostgreSQL / Aurora MySQL peuvent invoquer Lambda depuis la base. Les RDS event notifications portent sur l'instance, pas sur les données (SNS/EventBridge).
- Nouveautés : Lambda Managed Instances (EC2 géré par Lambda), durable functions (checkpoint/reprise). Bon à connaître ; l'examen reste sur les limites classiques.
- **Edge** : CloudFront Functions (JS ; millions req/s ; < 1 ms ; 2 MB ; sans réseau/fichiers ; ~1/6 du coût de Lambda@Edge ; viewer request/response ; normalisation de cache key, headers, rewrites d'URL, auth simple) vs Lambda@Edge (Node.js/Python ; milliers req/s ; jusqu'à 30 s ; 128 MB viewer, jusqu'à 10 GB origin ; réseau + fichiers ; viewer et origin ; logique longue, services externes, body).

## Limites et quotas

- Mémoire 128 MB à 10 240 MB (CPU proportionnel).
- Timeout **900 s (15 min)**, non extensible.
- /tmp 512 MB à 10 240 MB.
- Variables d'environnement 4 KB.
- Concurrency 1 000 par région par défaut (augmentable ; nouveaux comptes plus bas).
- Package 50 MB zippé, 250 MB décompressé ; images de conteneur jusqu'à 10 GB.
- Payload : 6 MB synchrone, 1 MB asynchrone, 200 MB en réponse streamée.

## Tarification (logique)

- Paiement par requête et par durée (incréments de 1 ms) ; free tier mensuel (1 million de requêtes, 400 000 GB-secondes).
- Provisioned concurrency : payant. SnapStart : sans coût.

## Quand l'utiliser vs alternatives

- Job > 15 min, > 10 GB RAM ou grosse image → ECS/Fargate ou AWS Batch ([[lambda-vs-batch]]). Orchestration → [[step-functions]]. API → [[api-gateway]].

## Pièges d'examen

- Un job de plus de 15 minutes ne tourne pas sur Lambda.
- Une fonction qui affame les autres = reserved concurrency.
- Cold starts = provisioned concurrency ou SnapStart.
- Lambda en VPC a besoin d'une NAT pour internet.
- Certificats edge-optimised dans us-east-1.

## Voir aussi

[[lambda-vs-batch]], [[api-gateway]], [[step-functions]], [[dynamodb]], [[rds]], [[sqs]], [[cloudfront]], [[ecs-fargate-ecr]]
