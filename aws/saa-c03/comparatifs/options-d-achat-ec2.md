---
tags:
  - aws
  - saa-c03
  - comparatif
  - cost
statut: a-revoir
source: notion
module-notion: "02. EC2 fundamentals"
---

# Options d'achat EC2

Choisir le bon modèle d'achat selon la durée, la flexibilité et la tolérance à l'interruption.

| Option | Remise vs On-Demand | Engagement | Convient à |
| --- | --- | --- | --- |
| On-Demand | Aucune | Aucun | Charges courtes, imprévisibles, ininterrompues |
| Standard Reserved Instances | Jusqu'à 72 % | 1 ou 3 ans (type, région, tenancy, OS) | Régime permanent (ex. base de données) |
| Convertible Reserved Instances | Jusqu'à 66 % | 1 ou 3 ans, changement de type/OS/famille | Régime permanent évolutif |
| Compute Savings Plans | Jusqu'à 66 % | Dépense/heure 1 ou 3 ans, toute famille/région/OS, aussi Fargate et Lambda | Long terme avec flexibilité |
| EC2 Instance Savings Plans | Jusqu'à 72 % | Dépense/heure, une famille dans une région | Long terme sur une famille |
| Spot | Jusqu'à 90 % | Aucun, mais reprise avec préavis de 2 min | Jobs tolérants aux pannes ; jamais une base critique |
| Dedicated Hosts | Variable | On-Demand ou réservé | Licences par socket/core, conformité stricte ; le plus cher |
| Dedicated Instances | Variable | Aucun | Matériel non partagé, sans contrôle du placement |
| On-Demand Capacity Reservations | Aucune | Aucun, facturé utilisé ou non | Garantir la capacité dans une AZ ; combiner avec Savings Plans/RI régionaux |

## Règle de décision

- Job interruptible → Spot (fleet price-capacity-optimized).
- Régime permanent → RI / Savings Plans ; besoin de flexibilité → Compute Savings Plans.
- Licences par core → Dedicated Host.
- Capacité garantie sans remise → Capacity Reservation.

## Pièges d'examen

- Capacity Reservation = capacité, pas de remise.
- Spot jamais pour une base critique.

## Voir aussi

[[ec2]], [[gestion-des-couts]]
