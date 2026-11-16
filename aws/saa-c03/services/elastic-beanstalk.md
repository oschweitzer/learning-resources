---
tags:
  - aws
  - saa-c03
  - performant
  - cost
statut: a-revoir
source: notion
module-notion: "21. Architecture patterns, other services and Well-Architected"
---

# AWS Elastic Beanstalk

## À quoi ça sert

PaaS orienté développeur : déploie votre code sur EC2, ASG, ELB, RDS et gère capacité, load balancing, scaling, monitoring. Vous gardez le contrôle des ressources sous-jacentes.

## Concepts clés

- **Gratuit**, on paie les ressources créées.
- Composants : une *application* contient des *application versions* et des *environnements* (dev, test, prod).
- Tiers : *web server tier* derrière un ELB ; *worker tier* qui traite les messages d'une file SQS et scale sur la profondeur. Le tier web peut pousser du travail au tier worker.
- Modes de déploiement : *single instance* (dev) ; *high availability avec load balancer* (prod).
- Plateformes : Go, Java, .NET, Node.js, PHP, Python, Ruby, Packer, Docker (mono et multi-conteneur).
- Lancer vite une appli : golden AMI + user data (c'est ce que fait Beanstalk).

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Gratuit ; paiement des ressources créées.

## Quand l'utiliser vs alternatives

- Déployer du code sans gérer l'infra mais en gardant la main → Beanstalk ; infra as code explicite → [[cloudformation]].

## Pièges d'examen

- Beanstalk est gratuit.
- Worker tier = SQS.

## Voir aussi

[[cloudformation]], [[sqs]], [[elb]], [[auto-scaling]]
