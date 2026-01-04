---
layout: page
title: Verifier-Guided Multi-Agent Orchestrator
description: Training LLM orchestrators for PDDL planning using compiler feedback as dense supervision
img:
importance: 1
category: work
---

## Verifier-Guided Multi-Agent Orchestrator Training for LLM Planning (2025)

This project develops a multi-agent LLM framework for PDDL planning, proposing two novel orchestrator training approaches that leverage compiler feedback as dense supervision signal.

### Key Contributions

- **Contextual Action Ranking (Supervised)**: A supervised learning approach where the orchestrator learns to rank and route actions to specialized agents based on problem context
- **RLVR with Intermediate Verification Rewards**: Reinforcement learning approach using verification rewards at intermediate steps, not just final outcomes

### Results

- Reduced total LLM calls (orchestrator + agents) by **40%** through improved routing decisions
- Trained Llama-3-8B orchestrator matching prompted GPT-4 performance at significantly lower inference cost
- Demonstrated effective use of symbolic verification as training signal for neural orchestrators

### Technical Details

The framework combines:
- Multi-agent architecture with specialized agents for different planning subtasks
- Compiler/verifier feedback integrated into the training loop
- Efficient orchestration reducing both latency and cost compared to single-agent approaches
