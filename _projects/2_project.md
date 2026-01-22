---
layout: page
title: GammaZero
description: GNN framework for scalable POMDP planning with uncertainty-aware graph representations
img:
importance: 2
category: work
---

## GammaZero: Learning to Guide Belief-Space Search for Long-Horizon POMDPs (2025)

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

[Paper (arXiv)](https://arxiv.org/abs/2510.14035) | [Project Website](https://common-sense-for-seq-decision-making.github.io/GammaZero_Learning_To_Guide_POMDP_Belief_Space_Search_With_Graph_Representations/)
