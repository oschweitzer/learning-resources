---
tags:
  - aws
  - saa-c03
  - resilient
  - cost
statut: a-revoir
source: notion
module-notion: "20. Disaster recovery, migration and multi-region"
---

# Migration de serveurs : 7 R, MGN, Application Discovery, VMware

## À quoi ça sert

Planifier et exécuter la migration de charges (serveurs, VM, gros volumes de données) vers AWS.

## Concepts clés

- **Les 7 R** : *Retire* (éteindre), *Retain* (garder on-premises), *Relocate* (sans changement au niveau hyperviseur : VMware Cloud on AWS), *Rehost* (lift & shift vers EC2 : MGN), *Replatform* (lift, tinker & shift : base vers RDS, appli vers Beanstalk), *Repurchase* (SaaS), *Refactor* (cloud-native : serverless, microservices).
- **Application Discovery Service** : collecte des données sur les serveurs on-premises. *Agentless* (inventaire VM, config, historique de perf CPU/mémoire/disque) ; *agent-based* (+ config système, processus, connexions réseau → **mapping des dépendances**).
- **Application Migration Service (MGN)** : service de rehost, successeur de CloudEndure Migration et de Server Migration Service ; réplication continue de serveurs physiques, virtuels, cloud ; cutover avec downtime et coût minimaux.
- **VM Import/Export** : VM existantes vers EC2 et retour ; les images Amazon Linux peuvent tourner en VM on-premises (VMware, KVM, VirtualBox, Hyper-V). **VMware Cloud on AWS** : étend un data center vSphere vers AWS avec les outils VMware (migrer, produire sur cloud privé/public, DR).
- **Gros volumes (exemple 200 TB sur 100 Mbps)** : internet/VPN = immédiat à mettre en place, ~185 jours de transfert ; Direct Connect 1 Gbps = ~1 mois de mise en place puis ~18,5 jours ; Snowball (clients existants) = 2 à 3 appareils en parallèle, ~1 semaine de bout en bout, combinable avec DMS ; réplication continue après le bulk via VPN ou DX avec DMS ou DataSync. Nouveaux clients : DataSync (réseau) ou Data Transfer Terminal (physique) car Snowball Edge est fermé depuis novembre 2025.

## Limites et quotas

- 200 TB à 100 Mbps ≈ 185 jours ; Direct Connect 1 Gbps ≈ 18,5 jours après ≈ 1 mois de mise en place.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Rehost de serveurs → MGN ; bases → [[dms-et-migration]] ; données → [[transfert-de-donnees]] ; DR de serveurs → [[dr-et-multi-region]].

## Pièges d'examen

- Lift & shift de serveurs = Application Migration Service.
- Mapper les dépendances = Application Discovery agent-based.

## Voir aussi

[[dms-et-migration]], [[transfert-de-donnees]], [[dr-et-multi-region]], [[elastic-beanstalk]]
