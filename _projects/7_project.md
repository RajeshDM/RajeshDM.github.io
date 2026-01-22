---
layout: page
title: Diffusion for Motion Planning
description: Using denoising diffusion models to generate collision-free paths in cluttered environments
img:
importance: 7
category: work
---

## Denoising Diffusion Probabilistic Models for Motion Planning (2024)

Developed an innovative solution using diffusion models to efficiently generate paths for objects in cluttered environments, overcoming limitations of traditional sampling-based methods.

### Key Results

- **80% generation accuracy** for collision-free paths in complex 2D environments
- **3x speedup** over RRT for paths longer than 50 steps
- Handles diverse obstacle shapes and configurations

### Technical Approach

- Applied denoising diffusion probabilistic models (DDPM) to path generation
- Model learns distribution of valid paths from training data
- Generates complete trajectories in parallel rather than incremental sampling
- Effective for long-horizon planning where traditional methods struggle

### Advantages Over Traditional Methods

Unlike Rapidly-exploring Random Trees (RRT) which can get trapped in narrow passages or require many samples for long paths, the diffusion approach generates complete paths directly, enabling faster planning in challenging scenarios.
