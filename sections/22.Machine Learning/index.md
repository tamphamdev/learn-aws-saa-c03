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

## Amazon Translate

- Natural and accurate language translation
- Amazon Translate allows you to localize content - such as websites and applications - for international users, and to easily translate large volume of text efficiently

## Amazon Comprehend

- For **Natural Language Processing - NLP**
- Fully managed and serverless service
- Uses machine learning to find insights and relationships in text
    - Language of the text
    - Extracts key phrases, places, people, brands, or events
    - Understands how positive or negative the text is
    - Analyzes text using tokenization and parts of speech
    - Automatically organizes a collection of text files by topic
- Sample use cases:
    - Analyze customer interactions (emails) to find what leads to a positive or negative experience
    - Create and groups articles by topics that Comprehend will uncover

### Comprehend Medical

- Amazon Comprehend Medial detects and returns useful information in unstructured clinical text:
    - Physician’s notes
    - Discharge summaries
    - Test results
    - Case notes
- Uses NLP to detect Protected Health Information (PHI) - DetechPHI API
- Store your documents in Amazon S3, analyze real-time data with Kinesis Data Firehose, or use Amazon Transcribe to transcribe patient narratives into text that can be analyzed by Amazon Comprehend Medical

## Amazon SageMaker

- Fully managed service for developers / data scientists to build ML models
- Typically, difficult to do all the processes in one place + provision servers
- Machine learning process (simplified): predicting your exam score

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c77497c-8d1e-480b-b9a1-d3dadc736e9b/Untitled.png)

## Amazon Forecast

- Fully managed service that uses ML to deliver highly accurate forecasts
- Example: predict the future sales of a raincoat
- 50% more accurate than looking at the data itself
- Reduce forecasting time from months to hours
- Use case: Product Demand Planning, Financial Planing, Resource Planning…

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8794f0d2-4cdf-4bf8-bd9e-a35b18ff97eb/Untitled.png)

## Amazon Kendra

- Fully managed **document search service** powered by Machine Learning
- Extract answers from within a document (text, pdf, HTML, PowerPoint, MS Word, FAQs…)
- Natural language search capabilities
- Learn from user interactions/feedback to promote perferred results **(Incremental Learning)**
- Ability to manually fine-tune search results (importance of data, freshness, custom…)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4025993d-156c-4439-951e-acaee647aa8f/Untitled.png)