---
layout: page
title: Hierarchical Object-Oriented POMDP
description: Hierarchical planning for object rearrangement in partially observable environments
img:
importance: 5
category: work
related_publications: true
---

## Hierarchical Object-Oriented POMDP for Object Rearrangement (2024)

This project addresses the challenge of object rearrangement in partially observable multi-object environments through hierarchical decomposition.

**Published at NeurIPS SpaVLE 2025**

### Key Results

- **71% task completion** in partially observable multi-object environments
- **2-3x improvement** over baselines through hierarchical decomposition
- **Scalability**: Handles environments with 20 objects across 4 rooms while baselines limited to 5 objects in 1 room

### Technical Approach

- Abstract planner reasons about high-level goals and object dependencies
- Learned low-level policies handle execution and complex constraints
- Object-oriented state representation enables generalization across object types

### Challenging Scenarios Handled

- Blocked paths requiring obstacle removal
- Object dependencies (must move A before B)
- Goal conflicts requiring replanning
- Partial observability requiring active exploration

The hierarchical approach enables tractable planning in scenarios where flat approaches fail due to combinatorial explosion.

[Paper (arXiv)](https://arxiv.org/pdf/2412.01348) | [Project Website](https://learning-for-seq-decision-making.github.io/HOO_POMDP_Hierarchical_Object_Oriented_POMDP_Planning_for_Object_Rearrangement/)
