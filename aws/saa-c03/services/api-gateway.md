---
tags:
  - aws
  - saa-c03
  - performant
  - secure
statut: a-revoir
source: notion
module-notion: "12. Serverless"
---

# Amazon API Gateway

## À quoi ça sert

Porte d'entrée serverless pour APIs : versions, stages, authentification, clés API, usage plans, throttling, cache, validation, OpenAPI, WebSocket.

## Concepts clés

- Types : **REST** (le plus de fonctions), **HTTP** (moins cher, latence plus basse, moins de fonctions), **WebSocket**.
- Intégrations : Lambda (REST serverless classique) ; backends HTTP (on-premises, ALB) pour ajouter rate limiting, cache, auth, clés ; services AWS directement (SendMessage SQS, PutRecord Kinesis, démarrage Step Functions) sans Lambda.
- **Endpoints** : *edge-optimised* (défaut, clients globaux via CloudFront, API dans une région) ; *regional* (clients de la même région ; CloudFront perso possible) ; *private* (depuis votre VPC via interface VPC endpoint + resource policy).
- **Sécurité** : IAM (SigV4) pour appelants internes/AWS ; **Cognito user pools** pour utilisateurs externes ; **Lambda authorizers** pour logique custom (jetons tiers) ; WAF possible sur REST ; domaines custom avec ACM (edge-optimised : certificat dans **us-east-1** ; regional : région de l'API) puis CNAME/alias Route 53.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- API serverless → API Gateway + [[lambda]] ; authentification utilisateurs → [[cognito]] ; cache API → voir [[architectures-de-reference]].

## Pièges d'examen

- Certificat d'un endpoint edge-optimised dans us-east-1.
- API privée = interface VPC endpoint + resource policy.

## Voir aussi

[[lambda]], [[cognito]], [[acm]], [[waf-shield-firewall-manager]], [[step-functions]]
