---
tags:
  - aws
  - saa-c03
  - resilient
statut: a-revoir
source: notion
module-notion: "10. Decoupling"
---

# Amazon MQ

## À quoi ça sert

Broker de messages managé pour RabbitMQ et ActiveMQ (protocoles ouverts : MQTT, AMQP, STOMP, OpenWire, WSS).

## Concepts clés

- À utiliser pour migrer une application on-premises qui utilise déjà ces protocoles, sans la réécrire pour SQS/SNS.
- **Pas serverless** : tourne sur des serveurs, ne scale pas comme SQS/SNS. Propose files et topics.
- Haute dispo : broker actif + standby dans 2 AZ avec stockage partagé (EFS pour ActiveMQ).

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Appli existante AMQP/MQTT → Amazon MQ ; nouvelle appli cloud-native → [[sqs]] / [[sns]].

## Pièges d'examen

- Applis AMQP ou MQTT existantes = Amazon MQ.

## Voir aussi

[[sqs]], [[sns]], [[sqs-vs-sns-vs-kinesis]]
