---
layout: page
title: GNN-Based Action Ranking
description: Graph neural networks for learning to rank actions in classical planning
img:
importance: 4
category: work
related_publications: true
---

## GNN-Based Action Ranking for Classical Planning (2023)

This project develops a learning-to-rank approach for guiding classical planners using graph neural networks, achieving strong generalization with minimal training data.

**Published at NeurIPS 2025**

### Key Results

- **89-100% success rates** with strong out-of-distribution generalization
- **75% less training data** required compared to value-function methods
- **13x better coverage** on challenging problems vs. learning value-based methods
- Generalizes to test problems **8x larger** than training problems
- **Outperforms LLMs** (GPT-O3, Gemini-2.5-Pro) by 20-30x in coverage and 4-5x in plan quality

### Technical Approach

- Action-centric graph representation capturing planning state structure
- Graph neural network combined with autoregressive GRU decoder for action sequence prediction
- Learning to rank formulation rather than value estimation

### Why Action Ranking?

Traditional approaches learn value functions or policies, which require extensive training data. Our ranking approach focuses on relative action quality, enabling efficient learning from smaller datasets while achieving better generalization.

[Paper (arXiv)](https://arxiv.org/pdf/2412.04752) | [Project Website](https://common-sense-for-seq-decision-making.github.io/GABAR_Graph_based_action_ranking_for_planning/)
