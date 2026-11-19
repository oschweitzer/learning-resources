# Anki cards: AWS SAA-C03

File: [`aws-saa-c03.tsv`](aws-saa-c03.tsv) (641 cards). Generated from my Notion notes: one piece of information per card, preferably decision cards, quotas and limits.

## Contents

- Deck: `AWS::SAA-C03`.
- Tags: `aws`, `saa-c03`, the service or comparison, and the domain (`secure`, `resilient`, `performant`, `cost`).
- GUIDs are stable (SHA-1 hash of the front): re-importing the file updates existing cards instead of creating duplicates.

## Note types used

| Note type | Fields | TSV columns |
| --- | --- | --- |
| `Basic (with Why)` | Front, Back, Why | 4, 5, 7 |
| `Cloze` | Text, Back Extra | 4, 5 |

Columns: 1 GUID, 2 note type, 3 deck, 4 front / text, 5 back / extra, 6 tags, 7 why.

## One-time setup

The `Basic (with Why)` note type does not exist by default:

1. Anki: *Tools > Manage Note Types > Add > Add: Basic*; name it `Basic (with Why)`.
2. *Fields*: add a third field `Why` (the first two are `Front` and `Back`).
3. *Cards > Back Template*: append `{{#Why}}<hr>{{Why}}{{/Why}}` at the end.

`Cloze` is the built-in cloze note type.

## Import

1. *File > Import*, choose `aws-saa-c03.tsv`.
2. The `#separator`, `#html`, `#guid column`, `#notetype column`, `#deck column`, `#tags column` headers are read automatically.
3. Check that the fields are mapped as in the table above, then *Import*.
4. For later imports, choose "Update existing notes".

Alternative without a custom note type: import as `Basic` and ignore the `Why` column.
