---
layout: fullhtml-post
title: "Epilogue: Open Questions"
date: 2026-05-14
categories: ["Planning under Uncertainty"]
tags: ["planning", "pomdp", "research"]
description: "What HOO-POMDP and GammaZero don't yet do, where the next decade of partially-observable planning is heading, and how this all connects to the rest of AI. Closer of the Planning Under Uncertainty series."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml h1 { font-size: 2em; line-height: 1.2; margin-top: 1em; }
  .blog-fullhtml h2 { font-size: 1.45em; margin-top: 2em; color: #222; }
  .blog-fullhtml h3 { font-size: 1.15em; margin-top: 1.4em; }
  .blog-fullhtml .subtitle { font-size: 1.05em; color: #555; font-style: italic; margin-top: -0.5em; }
  .blog-fullhtml hr { border: none; border-top: 1px solid #ddd; margin: 2em 0; }
  .blog-fullhtml code { background: #f4f4f4; padding: 2px 5px; border-radius: 3px; font-size: 0.9em; }
  .blog-fullhtml em { color: #333; }

  .blog-fullhtml .series-nav {
  background: #ecedfa; border-left: 4px solid #5b6abf; border-radius: 0 8px 8px 0;
  padding: 14px 18px; margin: 0 0 24px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; font-size: 0.92em;
  }
  .blog-fullhtml .series-nav strong { color: #3d4a9e; }
  .blog-fullhtml .series-nav .series-nav-links { margin-top: 6px; font-size: 0.85em; color: #555; }
  .blog-fullhtml .series-nav a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }
  .blog-fullhtml .series-nav a:hover { color: #28327a; }

  .blog-fullhtml .series-footer {
  margin: 3em 0 2em; padding: 20px 22px;
  background: #f5f6fc; border: 1px solid #d4d8ee; border-radius: 10px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .series-footer strong { color: #3d4a9e; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }

  .blog-fullhtml .open-q {
  margin: 20px 0; padding: 16px 20px;
  background: #fff; border-left: 4px solid #C5A55A; border-radius: 0 8px 8px 0;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .open-q strong { color: #C5A55A; }
  .blog-fullhtml .open-q p { font-size: 0.92em; color: #2D2044; line-height: 1.55; margin: 5px 0; }
  .blog-fullhtml .open-q .label { font-size: 0.68rem; text-transform: uppercase; letter-spacing: 1.5px; font-weight: 700; color: #C5A55A; margin-bottom: 4px; }

  .blog-fullhtml .strand {
  margin: 22px 0; padding: 18px 22px;
  background: #f5f6fc; border-radius: 10px; border: 1px solid #d4d8ee;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .strand h3 { font-family: 'Playfair Display', Georgia, serif; font-size: 1.05rem; color: #3d4a9e; margin: 0 0 8px; }
  .blog-fullhtml .strand p { font-size: 0.92em; color: #2D2044; line-height: 1.55; margin: 6px 0; }
  .blog-fullhtml .strand .tag { display: inline-block; font-size: 0.65rem; text-transform: uppercase; letter-spacing: 1.5px; font-weight: 700; padding: 2px 8px; border-radius: 12px; background: #5b6abf; color: #fff; margin-right: 8px; vertical-align: middle; }

  .blog-fullhtml .blog-container { max-width: 760px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #b8c8f0; border-bottom-color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #1a1f3a; border-left-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #15192e; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .open-q { background: #1e1a30; border-left-color: #C5A55A; }
  html[data-theme="dark"] .blog-fullhtml .open-q strong { color: #C5A55A; }
  html[data-theme="dark"] .blog-fullhtml .open-q p { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .open-q .label { color: #C5A55A; }
  html[data-theme="dark"] .blog-fullhtml .strand { background: #15192e; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .strand h3 { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .strand p { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Planning Under Uncertainty &middot; Epilogue</strong>
    <div class="series-nav-links">
        &larr; <a href="/blog/2026/planning-under-uncertainty-gammazero/">Part 4: GammaZero</a> &middot; Series start: <a href="/blog/2026/planning-under-uncertainty-overview/">Overview</a>
    </div>
</div>

<h1>Open Questions</h1>
<p class="subtitle">What HOO-POMDP and GammaZero don't yet do, where the next decade of partially-observable planning is heading, and how this all connects to the rest of AI.</p>

<hr>

<p>The series finished where the two papers do: HOO-POMDP shows you can shrink a POMDP through hierarchy until a principled solver handles it; GammaZero shows you can learn over a belief graph and transfer to larger problems. Both are real. Both leave a lot on the table.</p>

<p>This epilogue is the deliberate hand-wave at the table.</p>

<h2>Three things even the best current methods can't do</h2>

<div class="open-q">
    <div class="label">Open question 1</div>
    <p><strong>Joint reasoning across belief and action time-scales.</strong> HOO-POMDP factors object beliefs to keep tractable, but the abstraction is hand-designed (per domain). GammaZero learns the belief representation but inherits a fixed MCTS budget. Real agents need to <em>adaptively</em> spend belief tracking effort and search effort, trading them off. Currently no method does both well at scale.</p>
</div>

<div class="open-q">
    <div class="label">Open question 2</div>
    <p><strong>Continuous observation spaces with high information content.</strong> A robot looking at a kitchen counter receives an image, not a one-bit observation. POMCPOW and DESPOT-α partially handle continuous observations; GammaZero and HOO-POMDP currently assume discrete or low-dimensional ones. Closing this gap connects POMDP planning to vision-language models &mdash; the largest unsolved interface in robotics.</p>
</div>

<div class="open-q">
    <div class="label">Open question 3</div>
    <p><strong>Learning the abstraction.</strong> HOO-POMDP works because someone hand-designed the right hierarchy (objects, rooms, room-graphs). GABAR/GammaZero work because someone chose the right relational graph encoding. The next step is to <em>learn</em> both the abstraction and the policy from data &mdash; without sacrificing the size-generalization guarantees. Promising work uses contrastive learning over belief states, but no method yet matches the sample efficiency of a good hand-designed abstraction.</p>
</div>

<h2>Three threads to watch</h2>

<div class="strand">
    <h3><span class="tag">Thread 1</span>Foundation models as priors over plans</h3>
    <p>LLMs encode an enormous amount of weak prior knowledge about how the world tends to be arranged. "Cups go in cupboards. Books go on shelves." A POMDP solver that consumes those priors as <em>belief seeds</em>, then refines them through search, could dramatically reduce the number of observations needed before useful planning is possible. The interface is the open question. <a href="/blog/category/llms-automated-planning-and-agents/">Planning in the Era of LLMs</a> covers the FO version of this story; the PO version is just starting.</p>
</div>

<div class="strand">
    <h3><span class="tag">Thread 2</span>Diffusion-style planners over belief space</h3>
    <p>Recent work generates plans by denoising trajectories conditioned on goals. Applied to belief space, the same idea could generate <em>belief-trajectory plans</em> &mdash; sequences of belief states that satisfy a goal &mdash; with the planner sampling from a learned distribution rather than searching a tree. This trades MCTS's correctness guarantees for sampling speed, which is the right trade-off when MCTS is the bottleneck (and it usually is, for large POMDPs).</p>
</div>

<div class="strand">
    <h3><span class="tag">Thread 3</span>Online curriculum from real deployments</h3>
    <p>GammaZero trains on small synthetic POMDPs and transfers to larger ones, but the training distribution is hand-crafted. The next step is to fold real-world deployment data back into training &mdash; a curriculum that grows with the deployment fleet. Closely related to offline RL, but with the structure of POMDPs explicit in the loss. Operationally hard. Methodologically wide open.</p>
</div>

<h2>Where this connects to the rest of AI</h2>

<p>The narrow story of these four posts is "POMDPs are hard; here are two strategies that scale them." The broader story is that <strong>partial observability is the central technical obstacle to deploying capable agents in the physical world</strong>. The robots that exist today work around it &mdash; with engineered sensor suites, with closed environments, with humans in the loop. Real autonomy requires solving it, not engineering around it.</p>

<p>That's true for household robotics. It's true for autonomous driving. It's increasingly true for software agents (which have the digital equivalent of partial observability: a tool returns a result, but the underlying state of the external system is mostly hidden). The agents getting most of the spotlight in 2025-2026 &mdash; LLM-driven software agents &mdash; are not POMDP-native, and they pay for it in ways the field is still discovering.</p>

<p>This series is therefore not just about classical POMDP solving. It's about the algorithmic primitives that will underlie the next generation of agents that act under uncertainty &mdash; whether those agents are robots, software, or hybrids. The two paper anchors (HOO-POMDP, GammaZero) are concrete steps in that direction.</p>

<h2>Where to go next from here</h2>

<ul>
    <li><strong>If you want the FO version:</strong> <a href="/blog/2026/learning-for-planning-overview/">Learning for Planning</a>. Same recipe, easier setting. GammaZero (Post 4 here) is GABAR (Post 4 there) ported to belief states.</li>
    <li><strong>If you want the LLM-agent angle:</strong> <a href="/blog/category/llms-automated-planning-and-agents/">Planning in the Era of LLMs</a>. How modern LLM agents fail at planning, and the modern toolkit (PDDL-as-action-space, hybrid policies) for fixing them.</li>
    <li><strong>If you want the papers cold:</strong> <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">HOO-POMDP</a> and <a href="/blog/2026/planning-under-uncertainty-gammazero/">GammaZero</a>, both readable standalone.</li>
</ul>

<h2>One last note</h2>

<p>If you read both this series and the sibling <em>Learning for Planning</em> series end-to-end, you watched the same warehouse get a four-fold treatment: tractable / intractable / abstracted / learned, then again with fog: tractable-becomes-impossible / online-tree-search-buys-time / abstracted / learned. The fact that the same toy domain carries that much weight is not because the warehouse is special. It's because the underlying structural questions &mdash; how does the planner represent state? how does it scale? how does it handle partial information? &mdash; are the same questions whether you're delivering packages, rearranging a house, or guiding a software agent through an unfamiliar API.</p>

<p>Those questions are the work. Everything else is engineering.</p>

<hr>

<div class="series-footer">
    <strong>Series end</strong>
    <p>That closes <em>Planning Under Uncertainty</em>. The companion series &mdash; <a href="/blog/2026/learning-for-planning-overview/">Learning for Planning</a> &mdash; covers the same warehouse without fog. <a href="/blog/category/llms-automated-planning-and-agents/">Planning in the Era of LLMs</a> covers the LLM-agent dimension. All three live in the same repository and link to each other.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;">&larr; <a href="/blog/2026/planning-under-uncertainty-gammazero/">Part 4: GammaZero</a> &middot; Series start: <a href="/blog/2026/planning-under-uncertainty-overview/">Overview</a> &middot; Sibling: <a href="/blog/2026/learning-for-planning-overview/">Learning for Planning</a></p>
</div>

</article>
