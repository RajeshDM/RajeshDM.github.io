---
layout: page
title: GammaZero
description: GNN framework for scalable POMDP planning with uncertainty-aware graph representations
img:
importance: 2
category: work
---

## GammaZero: GNN Framework for Scalable POMDP Planning (2025)

GammaZero is a graph neural network-based framework for partially observable Markov decision process (POMDP) planning that achieves dramatic computational efficiency gains while enabling zero-shot transfer to larger scenarios.

### Key Contributions

- **20x Computational Efficiency**: Reduced inference time from minutes to seconds while maintaining planning accuracy
- **Zero-Shot Transfer**: Enables generalization to scenarios 6x larger than those seen during training
- **Uncertainty-Aware Graph Representation**: Novel graph representation that captures partial observability and belief state uncertainty

### Technical Approach

- Graph neural networks learn policy and value functions over belief-space representations
- Learned models integrated with Monte Carlo Tree Search (MCTS) to drastically reduce search requirements
- Uncertainty-aware node and edge features encode observation history and belief distributions

### Applications

The framework is applicable to robotic planning, autonomous navigation, and other domains requiring decision-making under uncertainty with scalability to real-world problem sizes.

<!-- Add paper link here when available -->
