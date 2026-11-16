---
tags:
  - aws
  - saa-c03
  - performant
  - cost
  - resilient
statut: a-revoir
source: notion
module-notion: "02. EC2 fundamentals"
---

# Amazon EC2 : instances, achat, placement

## À quoi ça sert

Serveurs virtuels : choisir la famille d'instance, l'option d'achat, le placement et le réseau.

## Concepts clés

- Config d'une instance : OS, CPU/RAM via le type, stockage (EBS, EFS ou instance store), security groups, IAM role, key pair, **user data**.
- **User data** : script de bootstrap exécuté **une seule fois**, en root, au tout premier démarrage.
- **IMDS** (169.254.169.254) expose infos d'instance et credentials de role. Imposer **IMDSv2** (tokens de session, bloque une classe d'attaques SSRF).
- Nom d'instance m7g.2xlarge = classe m, génération 7, attribut g (Graviton/ARM), taille 2xlarge. Attributs : a (AMD), i (Intel), d (NVMe local), n (réseau amélioré).
- Familles : général (t, m ; t = burstable avec CPU credits), compute (c), mémoire (r, x, z, u), stockage (i, d, h), accéléré (p, g, inf, trn). Graviton = souvent le meilleur rapport prix/perf si l'appli tourne sur ARM.
- **Security group** : firewall stateful attaché à une ENI, règles *allow* uniquement ; inbound refusé et outbound autorisé par défaut. Peut référencer une IP/CIDR ou un autre SG. Un timeout = problème de SG/NACL ; « connection refused » = l'application.
- Ports à connaître : 22 SSH/SFTP, 21 FTP, 80/443 HTTP(S), 3389 RDP, 3306 MySQL/MariaDB/Aurora MySQL, 5432 PostgreSQL/Aurora PostgreSQL, 1433 SQL Server, 1521 Oracle.
- Connexion : SSH (port 22), EC2 Instance Connect (port 22 ouvert quand même), **Session Manager** (aucun port, aucune clé, aucun bastion, sessions loguées → réponse préférée).
- **Spot** : préavis de 2 minutes avant reprise (stop ou terminate) → prévoir des checkpoints. Requête Spot persistante : annuler la requête **puis** terminer les instances. Spot Fleet / EC2 Fleet : stratégie recommandée `price-capacity-optimized`.
- **IP** : IP privée ; IPv4 publique change à chaque stop/start. **Elastic IP** = IPv4 publique statique, remappable (masquer une panne). Toutes les IPv4 publiques sont facturées, attachées ou non. Préférer DNS ou load balancer.
- **ENI** : carte réseau virtuelle, liée à **une AZ**, IP privée primaire + secondaires, une Elastic IP par IP privée, SGs, MAC. Déplaçable entre instances pour du failover.
- **Hibernate** : écrit la RAM sur le volume root EBS, redémarrage rapide avec processus et caches. Exige root EBS chiffré assez grand pour la RAM, RAM < 150 GB, pas de bare metal. Dure au maximum 60 jours.

## Limites et quotas

- Elastic IP : 5 par région par défaut.
- Placement group spread : 7 instances par AZ par groupe.
- Hibernation : 60 jours max, RAM < 150 GB.

## Tarification (logique)

- On-Demand : facturation à la seconde (Linux/Windows) après la première minute, aucun engagement.
- Plus l'engagement est long et le paiement anticipé (all upfront, 3 ans), plus la remise est forte.
- Capacity Reservation : facturée utilisée ou non, sans remise (à combiner avec Savings Plans ou RI régionaux).
- Dedicated Host : option la plus chère.

## Quand l'utiliser vs alternatives

- Options d'achat → [[options-d-achat-ec2]].
- Pour du conteneur sans serveur → [[ecs-fargate-ecr]] ; pour du code événementiel → [[lambda]].

## Pièges d'examen

- Spot : jamais pour une base critique ni un job non interruptible.
- Capacity Reservation = capacité garantie, pas de remise.
- Dedicated Host = licences par socket/core.
- User data ne s'exécute qu'au premier boot.

## Voir aussi

[[options-d-achat-ec2]], [[ebs]], [[ami-et-instance-store]], [[security-groups-et-nacl]], [[auto-scaling]], [[systems-manager]]
