# Cross-lingual-disinformation-experiments
## Overview
This repository contains the code and experiments investigating whether large language models (LLMs) produce consistent disinformation verdicts when the same claim is presented in different languages.

## Datasets Used
### CONSTRAINT 2021 — Hindi Hostile Post Detection
Source: GitHub
Description: 6,539 Hindi social media posts labelled with fine-grained hostility categories including fake, hate, offensive, and defamation. Collected for the CONSTRAINT shared task at AAAI 2021.
Used for: Primary claim source — 20 posts carrying the fake label were sampled as the experimental corpus.
### EUvsDisinfo
Source: euvsdisinfo.eu | Scraped GitHub mirror
Description: 14,497 verified pro-Kremlin disinformation cases maintained by the European External Action Service, covering 15 languages and updated continuously since 2015.
Used for: Experiment 6 — 234 cases targeting Estonia were extracted and sampled for comparison against translated Estonian claims.
### MM-COVID (planned, inaccessible)
Source: Paper | GitHub (crawler only)
Description: Multilingual COVID-19 fake news dataset across 6 languages with labels verified by Snopes and Poynter. Data files are no longer publicly accessible — only the crawler code remains.
Status: Replaced by CONSTRAINT 2021 after confirming inaccessibility.


## Model	Type	Notes
All models accessed via the OpenRouter unified API.
google/gemini-2.5-flash-lite	Closed, paid	Strong multilingual coverage
deepseek/deepseek-v4-flash	Closed, paid	MoE architecture, 13B active parameters
meta-llama/llama-3.3-70b-instruct:free	Open, free tier	Hit 200 req/day rate limit — results incomplete
qwen/qwen3-235b-a22b:free	Open, free tier	Hit 200 req/day rate limit — results incomplete

## Experiments
### Experiment 1 — Cross-Lingual Consistency

Tests whether LLM disinformation verdicts remain consistent when the same claim is presented in Hindi, English, and Estonian. Produces consistency rates per model and FAKE verdict rates per language.

### Experiment 2 — Instruction Language Effect

Tests whether the language of the prompt instruction — independent of claim language — changes model verdicts. Compares English, Hindi, and Estonian instruction languages across all claim-language combinations.

### Experiment 3 — Model Agreement Difficulty Map

Reuses Experiment 1 results to categorise claims by difficulty based on cross-model agreement. Produces a heatmap and a priority list for human annotation.

### Experiment 4 — Confidence Elicitation

Asks models to provide both a verdict and a confidence score (1–10). Measures whether confidence is well-calibrated across languages — i.e. whether high confidence correlates with high accuracy.

### Experiment 5 — Reasoning Audit

Extracts chain-of-thought reasoning responses and uses a classifier model to categorise them into error types: knowledge gap, correct reasoning, cultural blindspot, hallucination, excessive hedging, misclassification, and language difficulty.

### Experiment 6 — Native vs Translated Estonian

Compares model performance on translated Estonian claims (from Experiment 1) against natively sourced Estonian-targeted content from EUvsDisinfo. Finds that no native Estonian-language disinformation dataset currently exists.

### Experiment 7 — Retrieval-Augmented Prompting

For claims that all models missed, retrieves relevant Wikipedia context via DuckDuckGo and re-tests models with context appended to the prompt. Measures how often adding context corrects missed fake detections.
