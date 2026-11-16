---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "04. High availability and scalability"
---

# Elastic Load Balancing (ALB, NLB, GWLB, CLB)

## À quoi ça sert

Répartir le trafic sur plusieurs cibles, avec un DNS unique, des health checks, TLS, stickiness et multi-AZ. ELB est managé (upgrades, scaling, disponibilité).

## Concepts clés

- **ALB** (L7) : HTTP/HTTPS/WebSocket/HTTP2/gRPC, routage par path, host, query string, header ; redirections ; DNS fixe, **pas d'IP statique**. Cibles : EC2, tâches ECS (dynamic port mapping), Lambda, IP privées (y compris on-premises).
- **NLB** (L4) : TCP/UDP/TLS, millions de req/s, ultra faible latence, **une IP statique par AZ** (Elastic IP possible), conserve l'IP source, supporte les security groups. Cibles : EC2, IP privées, un ALB.
- **GWLB** (L3) : paquets IP, GENEVE port 6081, passe le trafic par une flotte d'appliances tierces (firewalls, IDS/IPS, DPI).
- **CLB** : legacy (L4 et L7), un seul certificat.
- Derrière un ALB, l'IP client est dans `X-Forwarded-For` (+ `X-Forwarded-Port`, `X-Forwarded-Proto`).
- **Health checks** par target group (port + path, ex. /health) ; non-200 = unhealthy. Un NLB supporte TCP, HTTP, HTTPS.
- Pattern sécurité : SG du LB ouvre 80/443 à tous ; SG des instances n'autorise que le SG du LB.
- **Sticky sessions** (CLB, ALB, NLB) : cookies application (custom ou AWSALBAPP) ou durée (AWSALB, AWSELB pour CLB). Noms réservés : AWSALB, AWSALBAPP, AWSALBTG. Peut déséquilibrer ; mieux : appli stateless avec sessions dans ElastiCache/DynamoDB.
- **Cross-zone** : ALB activé par défaut (désactivable par target group, gratuit) ; NLB et GWLB désactivé par défaut (frais inter-AZ si activé) ; CLB désactivé par défaut, sans frais.
- **TLS** : certificat X.509, en général via ACM. **SNI** : plusieurs certificats sur un listener (ALB, NLB, CloudFront). CLB ne le supporte pas.
- **Deregistration delay** (connection draining sur CLB) : laisse finir les requêtes en vol. Défaut 300 s, plage 0 à 3 600 s, 0 = désactivé ; descendre à ~30 s pour des requêtes courtes.

## Limites et quotas

- Deregistration delay : 0 à 3 600 s (défaut 300).
- NLB : une IP statique par AZ.

## Tarification (logique)

- Cross-zone : ALB gratuit ; NLB/GWLB facturent les données inter-AZ si activé.

## Quand l'utiliser vs alternatives

- Comparaison détaillée → [[alb-vs-nlb-vs-gwlb]].
- IP statique devant une appli HTTP : NLB devant un ALB, ou [[global-accelerator]].

## Pièges d'examen

- Routage par path/host = ALB.
- IP statique, UDP ou perf extrême = NLB.
- Firewalls tiers = GWLB.
- Plusieurs certificats TLS sur un LB = ALB/NLB avec SNI.

## Voir aussi

[[alb-vs-nlb-vs-gwlb]], [[auto-scaling]], [[acm]], [[global-accelerator]], [[route-53]]
