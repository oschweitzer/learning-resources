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

# Amazon SQS

## À quoi ça sert

File de messages managée pour découpler des couches et absorber les pics (ex. 1 000 vidéos à encoder au lieu de 10).

## Concepts clés

- **Standard** : plus ancien service AWS, débit et nombre de messages illimités, rétention 4 jours par défaut (14 max), latence < 10 ms. Livraison **at-least-once** (doublons possibles), ordre au mieux → consommateurs **idempotents**.
- Messages jusqu'à **1 MiB** (depuis août 2025 ; avant 256 KiB). Plus gros : payload dans S3 + pointeur.
- Producteur : `SendMessage`. Consommateurs (EC2, on-premises, Lambda) : poll jusqu'à 10 messages, traitent, puis `DeleteMessage`. Plusieurs consommateurs en parallèle ; ASG scalé sur `ApproximateNumberOfMessagesVisible` (ou backlog par instance).
- **Visibility timeout** : 30 s par défaut ; si le message n'est pas supprimé à temps il redevient visible (traitement en double). `ChangeMessageVisibility` pour plus de temps. Trop haut : reprise lente après crash ; trop bas : doublons.
- **Long polling** (1 à 20 s, `ReceiveMessageWaitTimeSeconds`) : moins d'appels vides, moindre latence, toujours préféré au short polling. **Delay queue** : jusqu'à 15 min.
- **DLQ** : reçoit les messages échoués plus de `maxReceiveCount` fois. Une FIFO a besoin d'une DLQ FIFO. *Redrive* : renvoie vers la file source après correction.
- **FIFO** : ordre strict par *message group ID*, exactly-once via déduplication (ID ou content-based, fenêtre de 5 min) ; 300 msg/s (3 000 avec batching), le mode high throughput monte bien plus ; nom en `.fifo`.
- Sécurité : HTTPS, KMS au repos, chiffrement client optionnel ; IAM + SQS access policies (cross-account, ou pour laisser SNS/S3 écrire dans la file).

## Limites et quotas

- Rétention : 4 jours (défaut), 14 jours (max).
- Message : 1 MiB.
- Visibility timeout par défaut : 30 s.
- Long polling : 1 à 20 s.
- Delay queue : 15 min max.
- FIFO : 300 msg/s, 3 000 avec batching.
- Fenêtre de déduplication FIFO : 5 min.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Pull par des workers → SQS ; notification vers plusieurs abonnés → [[sns]] ; flux temps réel rejouable → [[kinesis-et-firehose]] ; protocoles AMQP/MQTT existants → [[amazon-mq]]. Comparatif → [[sqs-vs-sns-vs-kinesis]].

## Pièges d'examen

- Messages traités en double → augmenter le visibility timeout, rendre le traitement idempotent.
- Ordre + exactly-once = FIFO.
- Un événement → plusieurs consommateurs = fan-out SNS vers SQS.

## Voir aussi

[[sns]], [[sqs-vs-sns-vs-kinesis]], [[lambda]], [[auto-scaling]], [[kinesis-et-firehose]]
