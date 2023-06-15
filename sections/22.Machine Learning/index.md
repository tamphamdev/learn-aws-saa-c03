# Machine Learning

# Overview

## Amazon Rekognition

- Find objects, people, text, scenes in image and videos using ML
- Facial analysis and facial search to do user verification, people counting
- Create a database of “familiar faces” or compare against celebrities
- Use cases:
    - Labeling
    - Content Moderation
    - Text Detection
    - Face Detection and Analysis (gender, age range, emotions…)
    - Face Search and Verification
    - Celebrity Recognition
    - Pathing (ex: for sports game analysis)

### Content Moderation

- Detect content that is inappropriate, unwanted, or offensive (image and videos)
- Used in social media, broadcast media, advertising, and e-commerce situations to create a safer user experience
- **Set a Minimum Confidence Threshold for items that will be flagged**
- Flat sensitive content for manual review in Amazon Augmented AI (A2I)
- Help comply with regulations

## Amazon Transcribe

- Automatically **convert speech to text**
- Uses a **deep learning process** called **automatic speech recognition** (ASR) to convert speech to text quickly and accurately
- **Automatically remove Personally Identifiable Information (PII) using Redaction**
- **Supports Automatic Language Identification for multi-lingual audio**
- Use cases:
    - transcribe customer service calls
    - automate closed captioning and subtitling
    - generate metadata for media assets to create a fully searchable archive

## Amazon Polly

- Turn text into lifelike speech using deep learning
- Allowing you to create applications that talk

### Lexicon & SSML

- Customize the pronunciation of words with **Pronunciation lexicons**
    - Stylized words: St3ph4ne ⇒ “Stephane”
    - Acronyms: AWS ⇒ “Amazon Web Services”
- Upload the lexicons and use them in the **SynthesizeSpeech** operation
- Generate speech from plain text or from documents marked up with **Speech Synthesis Markup Language (SSML)** - enables more customization
    - emphasizing specific words or phrases
    - using phonetic pronunciation
    - including breathing sounds, whispering
    - using the Newscaster speaking style