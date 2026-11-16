---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "19. VPC networking"
---

# VPC Flow Logs, Traffic Mirroring et Network Firewall

## À quoi ça sert

Observer (Flow Logs, mirroring) et filtrer (Network Firewall) le trafic d'un VPC.

## Concepts clés

- **VPC Flow Logs** : métadonnées IP d'un VPC, subnet ou ENI (y compris services managés avec ENI : ELB, RDS, ElastiCache, Redshift, NAT gateway, Transit Gateway). Destinations : S3 (Athena), CloudWatch Logs (metric filters, alarmes, Contributor Insights), Firehose. CloudWatch Logs exige un IAM role (`logs:CreateLogGroup`, `CreateLogStream`, `PutLogEvents`). Champs : srcaddr, dstaddr, srcport, dstport, protocol, packets, bytes, start, end, action (ACCEPT/REJECT), log-status.
- **Dépannage** : requête entrante rejetée → NACL ou SG ; entrant ACCEPT mais sortant REJECT → NACL (SG stateful) ; sortant ACCEPT mais retour entrant REJECT → NACL.
- **Traffic Mirroring** : copie le trafic des ENI (tout ou filtré) vers une ENI ou un NLB avec des appliances de sécurité (inspection de contenu, menaces, troubleshooting) ; dans un VPC ou entre VPC peered.
- **Network Firewall** : protège tout un VPC de L3 à L7 dans tous les sens (VPC↔VPC, sortant, entrant, vers/depuis DX et VPN) ; GWLB en interne ; géré cross-compte par Firewall Manager ; milliers de règles : IP/port, blocage de protocole (ex. SMB sortant), listes stateful de domaines autorisés (`*.mycorp.com`), regex, inspection TLS ; actions allow/drop/alert ; logs vers S3, CloudWatch Logs, Firehose.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Diagnostiquer un blocage → Flow Logs ; filtrage par domaine sur tout un VPC → Network Firewall ; filtrage L7 d'une appli web → [[waf-shield-firewall-manager]].

## Pièges d'examen

- Flow Logs : inbound ACCEPT + outbound REJECT = NACL.

## Voir aussi

[[security-groups-et-nacl]], [[cloudwatch]], [[athena]], [[waf-shield-firewall-manager]], [[detection-et-conformite]], [[vpc]]
