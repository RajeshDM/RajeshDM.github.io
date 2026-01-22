---
layout: page
title: Prioritized DDPG
description: Enhanced reinforcement learning for continuous control with parameter noise exploration
img:
importance: 9
category: work
related_publications: true
---

## Prioritized DDPG (PDDPG) with Noise for Continuous RL (2019)

**Published at ISNN 2019**

Designed a prioritized form of Deep Deterministic Policy Gradient (DDPG) that outperforms standard approaches on continuous control benchmarks.

### Key Results

- **Outperformed DDPG** on 80% of MuJoCo benchmarks (8 out of 10 environments)
- **40% faster convergence** using parameter space noise exploration
- Continued improvement beyond 2000 iterations where standard DDPG plateaued

### Technical Contributions

- Combined prioritized experience replay with DDPG
- Implemented parameter space noise for exploration (more effective than action space noise)
- Demonstrated improved sample efficiency in continuous action spaces

### Why Parameter Space Noise?

Traditional action space noise adds randomness to selected actions, but parameter space noise perturbs the policy network weights directly. This leads to more consistent and state-dependent exploration, resulting in faster learning and better final performance.

The algorithm addresses the exploration-exploitation tradeoff in continuous control, a fundamental challenge in robotics and autonomous systems.
