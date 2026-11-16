# Cartes Anki : AWS SAA-C03

Fichier : [`aws-saa-c03.tsv`](aws-saa-c03.tsv) (640 cartes). Généré à partir de mes notes Notion : une information par carte, de préférence des cartes de décision, des quotas et des limites.

## Contenu

- Deck : `AWS::SAA-C03`.
- Tags : `aws`, `saa-c03`, le service ou le comparatif, et le domaine (`secure`, `resilient`, `performant`, `cost`).
- Les GUID sont stables (hash SHA-1 du recto) : réimporter le fichier met à jour les cartes existantes au lieu de créer des doublons.

## Types de notes utilisés

| Type de note | Champs | Colonnes du TSV |
| --- | --- | --- |
| `Basique (avec Pourquoi)` | Recto, Verso, Pourquoi | 4, 5, 7 |
| `Texte à trous` (cloze) | Texte, Texte supplémentaire | 4, 5 |

Colonnes : 1 GUID, 2 type de note, 3 deck, 4 recto / texte, 5 verso / extra, 6 tags, 7 pourquoi.

## Préparation (une seule fois)

Le type `Basique (avec Pourquoi)` n'existe pas par défaut :

1. Anki : *Outils > Gérer les types de notes > Ajouter > Ajouter : Basique* ; nom : `Basique (avec Pourquoi)`.
2. *Champs* : ajouter un troisième champ `Pourquoi` (les deux premiers s'appellent `Recto` et `Verso`).
3. *Cartes > Modèle du verso* : ajouter à la fin `{{#Pourquoi}}<hr>{{Pourquoi}}{{/Pourquoi}}`.

`Texte à trous` est le nom du type cloze dans Anki en français (`Cloze` en anglais). Si votre Anki est en anglais, renommer les types dans le fichier, ou renommer les types dans Anki.

## Importer

1. *Fichier > Importer*, choisir `aws-saa-c03.tsv`.
2. Les en-têtes `#separator`, `#html`, `#guid column`, `#notetype column`, `#deck column`, `#tags column` sont lus automatiquement.
3. Vérifier que les champs sont associés comme dans le tableau ci-dessus, puis *Importer*.
4. Mode de mise à jour : choisir « Mettre à jour les notes existantes » pour les imports suivants.

Alternative sans type personnalisé : importer en `Basique` et ignorer la colonne `Pourquoi`.
