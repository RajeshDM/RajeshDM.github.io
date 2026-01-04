---
layout: page
title: GlobXAI
description: LLM-based natural language interface for explainable AI over image classifiers
img:
importance: 3
category: work
---

## GlobXAI: LLM-Based Natural Language Interface for Explainable AI (2025)

GlobXAI enables users to query global explanations of image classifiers using natural language, bridging the gap between complex AI explanations and human understanding.

### Key Contributions

- **Natural Language Querying**: Users can ask questions about model behavior in plain English
- **Hybrid Architecture**: LLM parses queries while Pandas executes deterministically, ensuring hallucination-free outputs
- **High Accuracy**: Fine-tuned Gemma-2 9B achieves 98.7% parse rate, 82.5% exact-match accuracy, and 96.2% intent classification

### Technical Approach

- Fine-tuned Gemma-2 9B using QLoRA to convert natural language queries into structured JSON
- Structured queries executed deterministically over explanation databases using Pandas
- Two-stage architecture separates language understanding from data retrieval for reliability

### Example Queries

- "Which features are most important for classifying cats?"
- "How does the model distinguish between dogs and wolves?"
- "What patterns does the model rely on for this class?"

The system provides trustworthy answers by grounding LLM outputs in actual computed explanations rather than generating responses directly.
