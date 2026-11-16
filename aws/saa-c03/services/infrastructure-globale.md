---
tags:
  - aws
  - saa-c03
  - secure
statut: a-revoir
source: notion
module-notion: "01. Global infrastructure and IAM"
---

# Infrastructure globale AWS et responsabilité partagée

## À quoi ça sert

Choisir où déployer (région, AZ) et savoir qui est responsable de quoi.

## Concepts clés

- Une **region** est un cluster de data centers dans une zone géographique (ex. eu-west-3 Paris, us-east-1 N. Virginia). La plupart des services sont régionaux.
- Une **Availability Zone (AZ)** = un ou plusieurs data centers discrets (énergie, réseau, connectivité redondants). Isolées entre elles, reliées par un réseau à haut débit et faible latence.
- **Points of presence** (edge locations + regional edge caches) : plus de 400. Utilisés par CloudFront, Route 53, Global Accelerator.
- **Local Zones** : compute près des grandes villes. **Wavelength** : compute dans les réseaux 5G. **Outposts** : racks AWS dans votre data center.
- Critères de choix d'une région : compliance, latence, disponibilité des services, prix.
- Services globaux : IAM, Route 53, CloudFront, WAF (avec CloudFront). Services régionaux : EC2, Elastic Beanstalk, Lambda, Rekognition, la plupart du catalogue.
- **Responsabilité partagée** : AWS = sécurité *du* cloud (matériel, facilities, réseau, hyperviseur, parties managées). Vous = sécurité *dans* le cloud (données, IAM, chiffrement, security groups, patch OS sur EC2, code).
- La ligne bouge selon le service : EC2 → vous patchez l'OS ; RDS → AWS patche le moteur, vous gérez réseau, chiffrement, utilisateurs ; S3 → vous gérez bucket policies et chiffrement.

## Limites et quotas

- Une région a en général 3 AZ : jamais moins de 3, jusqu'à 6.

## Tarification (logique)

- Les prix diffèrent d'une région à l'autre (critère de choix).

## Quand l'utiliser vs alternatives

- Besoin de compute dans votre data center → Outposts ; dans la 5G → Wavelength ; près d'une grande ville → Local Zones.

## Pièges d'examen

- Les données ne quittent jamais une région sauf si vous les déplacez.
- Les nouveaux services/fonctions n'arrivent pas dans toutes les régions dès le premier jour.

## Voir aussi

[[iam]], [[services-divers]], [[dr-et-multi-region]]
