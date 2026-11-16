---
tags:
  - aws
  - saa-c03
  - performant
status: to-review
source: notion
notion-module: "15. Machine learning services"
---

# Managed machine learning services

## What it is for

Recognise which managed AI service matches a described need (use-case level, no need to know how to train models).

## Key concepts

- **Rekognition**: objects, people, text, scenes in images/videos; face analysis and search, celebrity recognition, content moderation.
- **Transcribe**: speech to text (ASR), PII redaction, automatic language identification. **Polly**: text to speech, pronunciation lexicons, SSML. **Translate**: neural machine translation.
- **Lex**: speech recognition + natural language understanding (the technology behind Alexa), chatbots/voice bots. **Connect**: cloud contact centre (calls, contact flows), integrates with Lex and CRMs.
- **Comprehend**: NLP (language, key phrases, entities, sentiment, topics). **Comprehend Medical**: clinical text, detects PHI.
- **SageMaker AI**: platform to build, train and deploy your own models. **Textract**: text, handwriting, forms and tables from scanned documents. **Kendra**: ML-powered enterprise document search with natural language questions. **Personalize**: real-time recommendations (out of scope in the current exam guide).
- Integrations: Aurora ML calls SageMaker AI/Comprehend from SQL; Transcribe -> Comprehend -> Translate; Rekognition moderation to human review with Amazon Augmented AI (A2I).

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Images/video -> Rekognition; scanned documents -> Textract; audio to text Transcribe; text to audio Polly; meaning of text Comprehend; language Translate; bot Lex; call centre Connect; search Kendra; own model SageMaker AI.

## Exam traps

- Textract reads documents; Rekognition reads images/video (text in a photo, not structured forms).
- Comprehend Medical, not Comprehend, for clinical PHI.

## See also

[[aurora]]
