---
tags:
  - aws
  - saa-c03
  - performant
  - secure
  - cost
statut: a-revoir
source: notion
module-notion: "08. CloudFront and Global Accelerator"
---

# Amazon CloudFront

## À quoi ça sert

CDN qui met en cache le contenu aux points of presence : meilleure performance de lecture, absorption DDoS, intégration Shield et WAF.

## Concepts clés

- **Origines** : *bucket S3* (sécuriser avec **Origin Access Control (OAC)** : bucket privé, policy qui n'autorise que la distribution) ; *VPC origin* (ALB, NLB ou EC2 privés, sans exposition internet) ; *custom origin HTTP* (site S3, ALB public, tout backend ; restreindre aux IP CloudFront ou header secret + WAF).
- Flux : l'edge regarde son cache, sur miss va à l'origine, met en cache pour le TTL, répond. **Origin groups** : failover primary/secondary.
- **Geo restriction** : allowlist ou blocklist de pays (licences, copyright).
- **Invalidation** : force le rafraîchissement (`/*` ou `/images/*`) ; des noms de fichiers versionnés évitent les invalidations.
- **CloudFront Functions** et **Lambda@Edge** personnalisent requêtes/réponses. **Signed URLs / signed cookies** pour contenu premium. Certificats ACM pour CloudFront : **us-east-1**.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- CloudFront en sortie est moins cher que S3 en sortie ; S3 → CloudFront est gratuit (voir [[vpc]] section coûts réseau).

## Quand l'utiliser vs alternatives

- Contenu cacheable partout → CloudFront. IP statiques, UDP, failover rapide → [[global-accelerator]]. Données dynamiques lues/écrites en peu de régions → S3 CRR. Comparaison → [[cloudfront-vs-global-accelerator]].

## Pièges d'examen

- Bucket S3 privé derrière CloudFront : OAC, pas de policy publique.
- Caching = CloudFront.
- Certificats CloudFront dans us-east-1.
- Bloquer une IP derrière CloudFront : WAF sur CloudFront, pas NACL.

## Voir aussi

[[s3]], [[global-accelerator]], [[cloudfront-vs-global-accelerator]], [[waf-shield-firewall-manager]], [[acm]], [[lambda]]
