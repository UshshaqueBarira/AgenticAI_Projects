WebLens
Structured Webpage Intelligence using LLMs
WebLens converts unstructured webpages into structured, decision-ready summaries using a multi-stage, production-oriented LLM pipeline.

Why WebLens Exists
Most webpages are written for humans to read slowly, not for humans to understand quickly.

Examples:

Government policy pages
Documentation
Job listings
Legal terms
Long articles

WebLens:

Scrapes raw webpage text
Validates content quality
Detects page intent
Normalizes information into a fixed schema
Prevents hallucinations by design
The result is consistent, explainable, and trustworthy structured output.

Architecture:
URL
 ↓
Scraper (HTML → text)
 ↓
Content Validator
 ↓
Page-Type Detector
 ↓
Chunker
 ↓
LLM Normalizer (schema enforced)
 ↓
Structured JSON Output

Module1:--------->scraper.py

Responsibility:
Fetches and cleans webpage content.

What it does
Downloads HTML using requests
Removes non-content tags (script, style, noscript)
Flattens content into plain text

Why it exists
LLMs cannot reason over HTML reliably.
This module guarantees clean string input.

Module2:-------->validators.py

Responsibility:
Ensures the page is worth processing.

What it does
Confirms input type is str
Rejects pages with insufficient content

Why it exists
Prevents hallucinations caused by thin or empty pages.
Example behavior
Marketing page → rejected
Short notice page → rejected
Policy / docs page → allowed

Module3:--------->page_type.py

Responsibility:
Detects the intent of the page.

Detected types
policy
documentation
job

general
Why it exists
Different page types require different reasoning strategies.
This is product logic, not prompt magic.

Module4:---------->chunker.py

Responsibility:
Splits long text into manageable segments.

What it does
Breaks text into 800–1200 word chunks

Why it exists
Prevents token overflow
Improves reasoning accuracy
Enables scalable processing


Module5:------------>llm_client.py

Responsibility:
Single interface to the LLM provider.

What it does
Sends prompts to OpenAI
Returns raw text output

Why it exists
Keeps model access isolated so it can be swapped or upgraded without touching business logic.

Module6:-------------->extractor.py

Responsibility:
Core intelligence layer.

What it does
Enforces a strict system prompt
Normalizes content into a fixed JSON schema
Forbids inference and hallucination
Parses model output into Python dictionaries

Why it exists
This is where:
Trust is enforced
Structure is guaranteed
Product behavior is defined


Module7:--------------->main.py

Responsibility:
Pipeline orchestration.

What it does
Connects all modules
Executes steps in correct order
Handles failure paths cleanly
Produces final output

Why it exists
Separates system control flow from business logic.


WebLens is a content normalization system, not a conversational tool.

Ran the code on 5-6 different websites
