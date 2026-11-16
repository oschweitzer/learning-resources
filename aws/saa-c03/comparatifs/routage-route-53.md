---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "06. Route 53"
---

# Les 8 politiques de routage Route 53

Le DNS ne route pas le trafic : la politique décide de la réponse.

| Politique | Comportement | Health checks | Cas d'usage |
| --- | --- | --- | --- |
| Simple | Une ou plusieurs valeurs, le client choisit au hasard | Non | Ressource unique |
| Weighted | Répartition par poids relatifs (0 = stop) | Oui | Migration progressive, canary |
| Latency | Région à la plus faible latence | Oui | Applis globales |
| Failover | Primary tant que sain, sinon secondary | Requis sur le primary | DR actif-passif |
| Geolocation | Continent, pays, état US ; record default nécessaire | Oui | Localisation, restrictions, compliance |
| Geoproximity | Distance aux ressources avec bias -99 à 99 | Oui | Déplacer du trafic entre régions |
| IP-based | CIDR du client (CIDR collection) | Oui | Routage par ISP, coût ou performance |
| Multi-value | Jusqu'à 8 valeurs saines | Oui | Répartition côté client ; pas un ELB |

## Règle de décision

- Latency ≠ geolocation : latence = région la plus rapide ; géo = où est l'utilisateur, quelle que soit la latence.

## Pièges d'examen

- Geolocation sans record default = utilisateurs non couverts sans réponse.
- Multi-value n'est pas un load balancer.

## Voir aussi

[[route-53]]
