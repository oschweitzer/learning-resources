---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "06. Route 53"
---

# Amazon Route 53

## À quoi ça sert

DNS managé et autoritatif, registrar de domaines et health checks. Seul service AWS avec un SLA de disponibilité de 100 %.

## Concepts clés

- Records : nom, type, valeur, routing policy, TTL. **A** (IPv4), **AAAA** (IPv6), **CNAME** (vers un autre hostname, interdit à l'apex), **NS**. Autres à reconnaître : CAA, MX, TXT, SOA, SRV, PTR, DS (l'examen teste surtout A, AAAA, CNAME, NS, alias).
- **Hosted zone** publique (internet) ou privée (une ou plusieurs VPC). NS et SOA créés automatiquement.
- **TTL** : élevé = moins de requêtes et de coût mais propagation lente ; bas = l'inverse. **Baisser le TTL avant une migration.** TTL obligatoire sauf pour les alias.
- **Alias** : vers une ressource AWS, autorisé à l'apex, requêtes gratuites, évaluation native de la santé de la cible, type A/AAAA. Cibles : LB, CloudFront, API Gateway, Beanstalk, site S3, VPC interface endpoints, Global Accelerator, records de la même zone. **Pas d'alias vers un nom DNS public EC2.**
- **Routing policies** : Simple, Weighted (poids relatifs, 0 = arrêt), Latency, Failover (health check requis sur le primary), Geolocation (continent/pays/état US ; record default nécessaire), Geoproximity (bias -99 à 99), IP-based (CIDR collections), Multi-value (jusqu'à 8 valeurs saines ; n'est pas un ELB).
- **Health checks** : endpoint (~15 checkers mondiaux, toutes les 30 s ou 10 s plus cher, seuil 3, sain si > 18 % des checkers d'accord, texte dans les 5 120 premiers octets), calculated (AND/OR/NOT, jusqu'à 256 enfants), CloudWatch alarm. Doivent pouvoir joindre l'endpoint (autoriser les IP des checkers). **Pour une ressource privée : métrique CloudWatch + alarme + health check sur l'alarme.**
- Domaine chez un registrar tiers : créer la hosted zone publique dans Route 53, mettre à jour les NS chez le registrar.
- **Resolver hybride** : *inbound endpoint* = on-premises résout des noms AWS ; *outbound endpoint* = VPC forwarde vers les résolveurs on-premises (règles par domaine). Nécessite VPN ou Direct Connect ; ENI dans 2 AZ.

## Limites et quotas

- Multi-value : 8 valeurs saines maximum.
- Health check calculated : 256 enfants max.
- Geoproximity bias : -99 à 99.

## Tarification (logique)

- Hosted zone facturée par mois ; domaines facturés à l'année.
- Requêtes alias gratuites (CNAME facturées).
- TTL bas = plus de requêtes = plus de coût.

## Quand l'utiliser vs alternatives

- CNAME vs alias → [[cname-vs-alias]]. Les 8 politiques → [[routage-route-53]]. Failover instantané indépendant du DNS → [[global-accelerator]].

## Pièges d'examen

- Un record à l'apex vers un LB = alias, jamais CNAME.
- Health checks ne joignent pas le privé : alarme CloudWatch.
- Geolocation exige un default record.
- Multi-value n'est pas un load balancer.

## Voir aussi

[[cname-vs-alias]], [[routage-route-53]], [[global-accelerator]], [[cloudfront]], [[acm]], [[vpc]]
