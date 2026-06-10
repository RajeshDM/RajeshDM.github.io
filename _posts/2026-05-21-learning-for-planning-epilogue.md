---
layout: fullhtml-post
title: "Epilogue: Toward Uncertainty"
date: 2026-05-21
categories: ["Learning for Planning"]
tags: ["planning", "gnn", "pomdp"]
description: "What GABAR doesn't do, why that matters, and where the natural next chapter takes the same warehouse. Closer of the Learning for Planning series."
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
  background: #e0f5f5; border-left: 4px solid #00A3A1; border-radius: 0 8px 8px 0;
  padding: 14px 18px; margin: 0 0 24px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; font-size: 0.92em;
  }
  .blog-fullhtml .series-nav strong { color: #00807E; }
  .blog-fullhtml .series-nav .series-nav-links { margin-top: 6px; font-size: 0.85em; color: #555; }
  .blog-fullhtml .series-nav a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }
  .blog-fullhtml .series-nav a:hover { color: #005856; }

  .blog-fullhtml .series-footer {
  margin: 3em 0 2em; padding: 20px 22px;
  background: #f0fafa; border: 1px solid #c2e4e3; border-radius: 10px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .series-footer strong { color: #00807E; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }

  .blog-fullhtml .bridge {
  margin: 28px -40px; padding: 26px 28px;
  background: linear-gradient(135deg, #e0f5f5, #ecedfa);
  border-radius: 14px; border: 1px solid #c2e4e3;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  @media (max-width: 800px) { .blog-fullhtml .bridge { margin: 24px 0; } }
  .blog-fullhtml .bridge h3 { font-family: 'Playfair Display', Georgia, serif; font-size: 1.2rem; color: #00807E; margin: 0 0 10px; }
  .blog-fullhtml .bridge p { font-size: 0.96em; color: #2D2044; line-height: 1.6; margin: 8px 0; }
  .blog-fullhtml .bridge .arrow { display: inline-block; font-family: 'JetBrains Mono', monospace; font-weight: 700; color: #5b6abf; }
  .blog-fullhtml .bridge a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; font-weight: 700; }
  .blog-fullhtml .bridge a:hover { color: #28327a; }

  .blog-fullhtml .open-q {
  margin: 20px 0; padding: 16px 20px;
  background: #fff; border-left: 4px solid #C5A55A; border-radius: 0 8px 8px 0;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .open-q strong { color: #C5A55A; }
  .blog-fullhtml .open-q p { font-size: 0.92em; color: #2D2044; line-height: 1.55; margin: 5px 0; }
  .blog-fullhtml .open-q .label { font-size: 0.68rem; text-transform: uppercase; letter-spacing: 1.5px; font-weight: 700; color: #C5A55A; margin-bottom: 4px; }

  .blog-fullhtml .blog-container { max-width: 760px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }
  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3a3a; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml a { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #0f2625; border-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #142a2a; border-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .bridge { background: linear-gradient(135deg, #142a2a, #1e1a3a); border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .bridge h3 { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .bridge p { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .bridge .arrow { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .bridge a { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .open-q { background: #1e1a30; }
  html[data-theme="dark"] .blog-fullhtml .open-q p { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Learning for Planning &middot; Epilogue</strong>
    <div class="series-nav-links">
        &larr; <a href="/blog/2026/learning-for-planning-gabar/">Part 4: GABAR</a> &middot; Next: <a href="/blog/2026/planning-under-uncertainty-overview/">Planning Under Uncertainty &rarr;</a>
    </div>
</div>

<h1>Toward Uncertainty</h1>
<p class="subtitle">What GABAR doesn't do, why that matters, and where the natural next chapter takes the same warehouse.</p>

<hr>

<p>If you read Posts 1-4 in order, you watched a tractable warehouse-delivery instance (4 zones, 1 package, solved in milliseconds by a classical planner) grow into ones that overwhelm classical search &mdash; and, in the paper's experiments, instances 8&times; larger than anything in training, with 100+ objects, solved in seconds by GABAR while the baselines collapse. The transition was smooth because we assumed one thing: <em>the robot can see everything</em>.</p>

<p>That's an unusual assumption. Most domains people actually want robots to operate in &mdash; warehouses included &mdash; don't grant that view. This epilogue is about what changes when you remove it, and what doesn't.</p>

<h2>What GABAR does well, and where it stops</h2>

<p>The pitch of Posts 1-4 was specific. With three architectural choices &mdash; action-centric graph, action ranking instead of value learning, conditional decoding of grounded actions &mdash; a GNN trained on small classical planning instances generalizes to instances 8&times; larger. That's a real result, and it's why GABAR is the series' anchor.</p>

<p>But the result lives entirely in the fully observable setting. The graph representation in Post 3 encodes <em>known</em> object positions and <em>known</em> predicate truth values. Action ranking in Post 2 assumes you can enumerate exactly which actions are available right now &mdash; which requires knowing the state. The whole stack collapses gracefully when state is uncertain&hellip; into something that needs a different framework.</p>

<p>Three things break:</p>

<div class="open-q">
    <div class="label">Open question 1</div>
    <p><strong>Object positions are unknown.</strong> In the running warehouse, the robot was assumed to know where every package was. What if it doesn't? The graph in Post 3 needs nodes that represent <em>distributions over positions</em>, not concrete positions. Edge weights now encode uncertainty.</p>
</div>

<div class="open-q">
    <div class="label">Open question 2</div>
    <p><strong>Actions produce observations, not just transitions.</strong> When the robot enters Zone C, it doesn't just change its position; it learns one bit of information about what's in Zone C. The action ranker now has to value information-gathering, not just goal-progress. "Go look at Zone C" might be the best action even if the goal is to deliver to Zone D.</p>
</div>

<div class="open-q">
    <div class="label">Open question 3</div>
    <p><strong>The state space explodes (again).</strong> Post 1's animation showed the fully observable state space hitting 1.2M reachable configurations. Adding partial observability multiplies that by the branching factor of observations at every step. The effective belief space is uncountable. We need MCTS-style online planning, not greedy execution.</p>
</div>

<h2>Where does this go?</h2>

<p>Two directions, both being actively pursued.</p>

<h3>Direction 1: abstract the problem until a principled solver can handle it.</h3>

<p>Pick a hierarchical decomposition. Object-oriented beliefs. Reason at the level of "where does each object roughly belong" rather than "what is the joint distribution over all positions." A POMDP solver like POMCP can then handle the smaller abstract problem, with grounded execution at the leaves. This is the strategy of <strong>HOO-POMDP</strong>, an earlier paper from our group.</p>

<h3>Direction 2: keep the problem big and learn to navigate the belief space.</h3>

<p>Apply the GABAR recipe to belief states. Same idea: state as a graph, action ranking as the target, GNN as the engine. But now the graph encodes a <em>belief</em> over states &mdash; with node features capturing uncertainty, and edge weights capturing probabilistic relations. Train on small POMDP instances; deploy on large ones. This is <strong>GammaZero</strong>, the partially-observable cousin of GABAR. It is, in some sense, what this series was building toward all along.</p>

<div class="bridge">
    <h3>Where the same warehouse goes next</h3>
    <p>The sibling series, <strong><a href="/blog/2026/planning-under-uncertainty-overview/">Planning Under Uncertainty</a></strong>, picks up the same 4-zone warehouse you've been reading about for four posts. Same robot. Same packages. Same action set.</p>
    <p>One change: <em>the robot can only see its current zone.</em></p>
    <p>That single change reframes everything. Post 1 of that series rebuilds the scaling visualization (now over belief states, climbing to 10<sup>15</sup>+); Post 2 introduces the online tree-search machinery (POMCP, DESPOT); Post 3 is the abstraction strategy (HOO-POMDP); and Post 4 is GammaZero &mdash; the GABAR-shaped paper for the partially-observable world.</p>
    <p><span class="arrow">&rarr;&nbsp;</span><a href="/blog/2026/planning-under-uncertainty-overview/">Start the Planning Under Uncertainty series</a></p>
</div>

<h2>Three further questions you might be asking</h2>

<h3>Does this generalize beyond classical planning?</h3>

<p>The recipe &mdash; "represent the state relationally, learn to rank what to do over the relational structure" &mdash; is more general than the planning setting. It shows up in chemistry, theorem proving, and combinatorial optimization. What's specific to classical planning is the PDDL framing of action schemas, which gives the GNN a particularly clean set of nodes to attend to. In domains without explicit action schemas, you have to choose what plays the role of the action-schema nodes. That choice is consequential.</p>

<h3>What about value functions, really?</h3>

<p>Post 2 made the case against value functions for generalization. That argument applies most forcefully when problem sizes differ between train and test. When sizes are fixed (chess, Atari, MuJoCo), value functions remain the right target. AlphaZero is right; GABAR doesn't argue with it. The argument is specifically about the planning generalization problem, where the input size varies.</p>

<h3>Is "small &rarr; large transfer" really the right framing?</h3>

<p>Possibly not, in the long run. The framing assumes the small problems contain enough signal to learn from. For domains where small instances are <em>structurally different</em> from large ones (some game playing, some real robotics), this is too optimistic. The future probably involves a mix of small-to-large transfer (where it works), curriculum learning (where small is a stepping stone but not the only signal), and offline RL (where there is no small-to-large at all). GABAR is the right baseline for the small-to-large story; it is not the right baseline for the others.</p>

<h2>The take-away in one paragraph</h2>

<p>You can train a graph neural network on solved instances of a classical planning problem, rank the actions available in each state, and deploy the network on instances orders of magnitude larger than what you trained on. The price of admission is a specific recipe: action ranking, action-centric graph, conditional decoding. None of these is novel in isolation. The combination, anchored on classical-planning structure, is what makes it work. The next chapter is the same recipe in a world where the robot can't see everything.</p>

<hr>

<div class="series-footer">
    <strong>Series end &middot; with a hand-off</strong>
    <p>That closes the <em>Learning for Planning</em> series. If you came this far, the natural next read is <a href="/blog/2026/planning-under-uncertainty-overview/">Planning Under Uncertainty</a> &mdash; the same warehouse and the same scaling story, this time with the robot's view fogged out.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;">&larr; <a href="/blog/2026/learning-for-planning-gabar/">Part 4: GABAR</a> &middot; Series start: <a href="/blog/2026/learning-for-planning-overview/">Overview</a> &middot; Next series: <a href="/blog/2026/planning-under-uncertainty-overview/">Planning Under Uncertainty &rarr;</a></p>
</div>

</article>
