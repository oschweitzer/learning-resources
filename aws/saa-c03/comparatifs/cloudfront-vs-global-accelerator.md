---
tags:
  - aws
  - saa-c03
  - comparatif
  - performant
  - resilient
statut: a-revoir
source: notion
module-notion: "08. CloudFront and Global Accelerator"
---

# CloudFront vs Global Accelerator

Tous deux utilisent le réseau mondial et les edge locations et s'intègrent à Shield.

|  | CloudFront | Global Accelerator |
| --- | --- | --- |
| Contenu | Cacheable (images, vidéo) et dynamique (accélération d'API), servi depuis l'edge | Pas de cache ; proxifie les paquets vers l'appli dans une ou plusieurs régions |
| Protocoles | HTTP et HTTPS | TCP ou UDP |
| Convient à | Sites, média, APIs | Jeux (UDP), IoT (MQTT), VoIP, apps HTTP avec IP statiques ou failover régional rapide |

## Règle de décision

- Cache → CloudFront.
- IP statiques / UDP / failover déterministe → Global Accelerator.

## Pièges d'examen

- Certificats CloudFront dans us-east-1.

## Voir aussi

[[cloudfront]], [[global-accelerator]]
