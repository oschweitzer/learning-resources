---
tags:
  - aws
  - saa-c03
  - performant
statut: a-revoir
source: notion
module-notion: "15. Machine learning services"
---

# Services de machine learning managés

## À quoi ça sert

Reconnaître quel service d'IA managé répond à un besoin décrit (niveau cas d'usage, pas besoin de savoir entraîner un modèle).

## Concepts clés

- **Rekognition** : objets, personnes, texte, scènes dans images/vidéos ; analyse et recherche de visages, célébrités, modération de contenu.
- **Transcribe** : parole → texte (ASR), redaction de PII, identification automatique de la langue. **Polly** : texte → parole, lexiques de prononciation, SSML. **Translate** : traduction neuronale.
- **Lex** : reconnaissance vocale + compréhension du langage (techno d'Alexa), chatbots/voicebots. **Connect** : centre de contact cloud (appels, contact flows), intégré à Lex et aux CRM.
- **Comprehend** : NLP (langue, phrases clés, entités, sentiment, sujets). **Comprehend Medical** : texte clinique, détecte les PHI.
- **SageMaker AI** : plateforme pour construire, entraîner et déployer ses propres modèles. **Textract** : texte, écriture manuscrite, formulaires et tables depuis des documents scannés. **Kendra** : recherche documentaire d'entreprise en langage naturel. **Personalize** : recommandations temps réel (hors scope du guide d'examen actuel).
- Intégrations : Aurora ML appelle SageMaker AI/Comprehend depuis SQL ; Transcribe → Comprehend → Translate ; modération Rekognition vers revue humaine avec Amazon Augmented AI (A2I).

## Limites et quotas

- Rien de précis dans mes notes Notion.

## Tarification (logique)

- Rien de précis dans mes notes Notion.

## Quand l'utiliser vs alternatives

- Images/vidéo → Rekognition ; documents scannés → Textract ; audio → texte Transcribe ; texte → audio Polly ; sens du texte Comprehend ; langue Translate ; bot Lex ; call center Connect ; recherche Kendra ; modèle perso SageMaker AI.

## Pièges d'examen

- Textract lit des documents ; Rekognition lit des images/vidéos (texte d'une photo, pas des formulaires structurés).
- Comprehend Medical, pas Comprehend, pour les PHI cliniques.

## Voir aussi

[[aurora]]
