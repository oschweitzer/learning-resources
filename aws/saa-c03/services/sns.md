---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
statut: a-revoir
source: notion
module-notion: "10. Decoupling"
---

# Amazon SNS

## À quoi ça sert

Pub/sub : un producteur publie une fois sur un topic, tous les abonnés reçoivent le message.

## Concepts clés

- Jusqu'à 12 500 000 abonnements par topic et 100 000 topics par compte.
- Abonnés : SQS, Lambda, Kinesis Data Firehose, endpoints HTTP(S), email, SMS, push mobile (APNS, FCM, ADM). Beaucoup de services AWS publient vers SNS (alarmes CloudWatch, ASG, S3, CloudFormation).
- Sécurité identique à SQS (HTTPS, KMS, IAM, access policies).
- **Message filtering** : filter policy JSON par abonnement ; sans filtre, l'abonné reçoit tout.
- **Fan-out SNS + SQS** : publier une fois, livrer à plusieurs files ; découplé, sans perte, chaque file à son rythme, ajout de files possible. L'access policy SQS doit autoriser le topic ; fonctionne cross-région. Cas S3 : une seule règle d'événement par type et prefix → SNS puis fan-out. SNS peut livrer à Firehose puis S3.
- **FIFO topics** : ordre par message group ID + déduplication ; abonnés SQS standard ou FIFO ; SNS FIFO + SQS FIFO = fan-out ordonné et dédupliqué.

## Limites et quotas

- 12,5 M d'abonnements par topic ; 100 000 topics par compte.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Notifier/fan-out → SNS ; workers qui consomment → [[sqs]] ; stream persistant → [[kinesis-et-firehose]]. Comparatif → [[sqs-vs-sns-vs-kinesis]].

## Pièges d'examen

- Un événement vers plusieurs consommateurs = SNS fan-out vers SQS.
- SNS ne persiste pas : un message non livré est perdu.

## Voir aussi

[[sqs]], [[sqs-vs-sns-vs-kinesis]], [[lambda]], [[s3]], [[eventbridge]]
