---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "03. EC2 storage"
---

# AMI et instance store

## À quoi ça sert

Booter vite et à l'identique (AMI) ; disposer de disques locaux très rapides mais éphémères (instance store).

## Concepts clés

- **AMI** : image avec OS, logiciels et configuration. **Régionale**, copiable entre régions. Sources : AMI publiques AWS, vos AMI, AWS Marketplace.
- Création : personnaliser une instance, la stopper (intégrité des données), créer l'AMI (crée aussi des snapshots EBS), lancer de nouvelles instances.
- **Golden AMI** : réduit le temps de boot, donc le cooldown d'un ASG.
- **Instance store** : disques physiquement attachés à l'hôte, IOPS et débit très élevés. **Éphémère** : données perdues au stop, terminate ou panne matérielle.
- Usages instance store : buffers, caches, données scratch, données répliquées. Sauvegardes et réplication = votre responsabilité.

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Besoin de IOPS locaux maximum, donnée perdable → instance store. Persistance → [[ebs]].

## Pièges d'examen

- Instance store : données perdues au stop.
- Une AMI est régionale (la copier pour une autre région).

## Voir aussi

[[ebs]], [[auto-scaling]], [[ec2]]
