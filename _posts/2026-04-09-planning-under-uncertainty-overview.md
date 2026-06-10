---
layout: fullhtml-post
title: "Planning Under Uncertainty — Series Overview"
date: 2026-04-09
categories: ["Planning under Uncertainty"]
tags: ["planning", "pomdp", "mcts"]
description: "A four-part series on planning when the agent can't see the full state — POMDPs, belief tracking, online tree search, and the two strategies (abstraction and learning) that make large partially-observable problems tractable."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml p { margin: 0 0 1.3em; }
  .blog-fullhtml ul, .blog-fullhtml ol { margin: 0 0 1.3em; }
  .blog-fullhtml h1 { font-size: 2.1em; line-height: 1.2; margin-top: 1em; }
  .blog-fullhtml h2 { font-size: 1.5em; margin-top: 2em; color: #222; }
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

  .blog-fullhtml .roadmap { margin: 24px 0; padding: 22px 24px 18px; background: #fff; border: 1px solid #d4d8ee; border-radius: 12px; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .roadmap h3 { font-family: 'Playfair Display', Georgia, serif; font-size: 1.1rem; color: #3d4a9e; margin: 0 0 14px; }
  .blog-fullhtml .rm-item { display: flex; gap: 14px; align-items: flex-start; padding: 12px 0; border-bottom: 1px solid #e8eaef; }
  .blog-fullhtml .rm-item:last-child { border-bottom: none; }
  .blog-fullhtml .rm-num { font-family: 'JetBrains Mono', monospace; font-size: 1.1rem; font-weight: 700; color: #5b6abf; flex-shrink: 0; min-width: 32px; }
  .blog-fullhtml .rm-body { flex: 1; }
  .blog-fullhtml .rm-title { font-family: 'Playfair Display', Georgia, serif; font-size: 1.05rem; color: #1a1a1a; font-weight: 700; }
  .blog-fullhtml .rm-title a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }
  .blog-fullhtml .rm-title a:hover { color: #28327a; }
  .blog-fullhtml .rm-desc { font-size: 0.9em; color: #555; margin-top: 3px; line-height: 1.55; }
  .blog-fullhtml .rm-tag { display: inline-block; font-size: 0.66rem; text-transform: uppercase; letter-spacing: 1.5px; font-weight: 700; color: #3d4a9e; background: #ecedfa; padding: 2px 8px; border-radius: 12px; margin-left: 6px; vertical-align: middle; }
  .blog-fullhtml .rm-tag.anchor { background: #5b6abf; color: #fff; }
  .blog-fullhtml .rm-tag.green { background: #d8e8df; color: #2e7d32; }

  .blog-fullhtml .pull { background: #3d4a9e; color: #fff; padding: 18px 22px; border-radius: 10px; margin: 28px 0; }
  .blog-fullhtml .pull p { font-size: 1.02em; line-height: 1.55; margin: 0; }
  .blog-fullhtml .pull strong { color: #ecedfa; }

  .blog-fullhtml .audience { display: flex; gap: 14px; margin: 22px 0; flex-wrap: wrap; }
  @media (max-width: 600px) { .blog-fullhtml .audience { flex-direction: column; } }
  .blog-fullhtml .aud-card { flex: 1; padding: 14px 16px; background: #f5f6fc; border-radius: 8px; border: 1px solid #d4d8ee; min-width: 180px; }
  .blog-fullhtml .aud-card h4 { font-family: 'Playfair Display', Georgia, serif; font-size: 0.98rem; color: #3d4a9e; margin: 0 0 6px; }
  .blog-fullhtml .aud-card p { font-size: 0.86em; color: #444; line-height: 1.5; margin: 0; }

  .blog-fullhtml .blog-container { max-width: 680px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3a3a; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #1a1f3a; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #15192e; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .roadmap { background: #1a1f3a; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .roadmap h3 { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .rm-item { border-bottom-color: #2a3045; }
  html[data-theme="dark"] .blog-fullhtml .rm-num { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .rm-title { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .rm-title a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .rm-title a:hover { color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .rm-desc { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .rm-tag { background: #1e2440; color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .rm-tag.anchor { background: #9aafe6; color: #0a1020; }
  html[data-theme="dark"] .blog-fullhtml .rm-tag.green { background: #1a2a1a; color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .pull { background: #2a3470; color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .pull strong { color: #c8d4ee; }
  html[data-theme="dark"] .blog-fullhtml .aud-card { background: #15192e; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .aud-card h4 { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .aud-card p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #9888a8; border-top-color: #2a3045; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Planning Under Uncertainty &middot; Series Overview</strong>
    <div class="series-nav-links">
        You are here. The series begins at <a href="/blog/2026/planning-under-uncertainty-belief-states/">Part 1 &rarr;</a>
    </div>
</div>

<h1>Planning Under Uncertainty</h1>
<p class="subtitle">A four-part series on planning when the agent can't see the full state &mdash; POMDPs, belief tracking, online tree search, and the two strategies (abstraction and learning) that make large partially-observable problems tractable.</p>

<hr>

<h2>Why this series exists</h2>

<p>Classical planning assumes you know everything. The sibling series, <a href="/blog/2026/learning-for-planning-overview/">Learning for Planning</a>, lived inside that assumption: same warehouse with a robot and packages, but the robot saw it all. That series spent four posts on the scaling problem alone.</p>

<p>Real agents almost never see everything. A robot tidying a house can't see through walls. A warehouse robot picking the next aisle can't see what's in the rest of the building. The state of the world is what it is &mdash; but the agent only ever has a probability distribution over it. That distribution is the agent's <em>belief</em>, and planning against a belief is much harder than planning against a state.</p>

<p>This series covers the formalism (POMDPs), the algorithmic toolkit the community converged on (POMCP, DESPOT, online tree search with particle filters), and the two parallel research paths that make large POMDPs solvable: <strong>abstraction</strong> (HOO-POMDP) and <strong>learning</strong> (GammaZero, the partially-observable cousin of GABAR from the LFP series).</p>

<div class="pull">
    <p><strong>Same warehouse, with fog.</strong> All four posts use the same warehouse from the LFP series &mdash; a robot delivering packages between zones A, B, C, D &mdash; with one change: <em>the robot can only see its current zone</em>. Post 1's animated counter shows what that change costs: the belief space, even before approximation, hits 10<sup>15</sup>+ effective configurations.</p>
</div>

<h2>The roadmap</h2>

<div class="roadmap">
    <h3>Four posts, two paper anchors, one foggy warehouse</h3>

    <div class="rm-item">
        <div class="rm-num">01</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/planning-under-uncertainty-belief-states/">Planning When You Can't See the Whole World</a> <span class="rm-tag">the setup</span></div>
            <div class="rm-desc">Belief states, Bayesian updates, and why partial observability is computationally brutal. Side-by-side animation: the same 16-zone warehouse, with and without fog. Counter climbs from 1K to 10<sup>15</sup>+ belief states. PSPACE-completeness, but visceral.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">02</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/planning-under-uncertainty-mcts-for-pomdps/">Online POMDP Solvers &mdash; A Survey</a> <span class="rm-tag">survey</span></div>
            <div class="rm-desc">The non-learning baselines GammaZero competes against: POMCP, DESPOT, POMCPOW, AdaOPS. Particle beliefs, the 4-phase MCTS loop, and the rollout-evaluation bottleneck that every one of them inherits. This is where the "we need learning" argument originates.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">03</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">HOO-POMDP &mdash; abstraction's ceiling</a> <span class="rm-tag anchor">paper deep-dive</span></div>
            <div class="rm-desc">Paper deep-dive. Hierarchical Object-Oriented POMDP for multi-object rearrangement scales to 20 objects via principled object-oriented belief factorization. But each decision still runs POMCP with random rollouts &mdash; nearly half an hour per task at 20 objects. This is the ceiling principled abstraction can reach with classical search inside, and the motivation for GammaZero.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">04</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/planning-under-uncertainty-gammazero/">GammaZero &mdash; learning past the ceiling</a> <span class="rm-tag anchor">paper deep-dive</span></div>
            <div class="rm-desc">Series finale and main paper deep-dive. Replaces POMCP's random rollouts with a learned value+policy network (the AlphaZero recipe for POMDPs). Crucially, the network is a GNN over a belief graph, so the same trained network handles POMDPs of any size &mdash; fixing BetaZero's fixed-dimensional bottleneck. Full lineage covered: AlphaZero, MuZero, BetaZero, ConstrainedZero, LeTS-Drive, GABAR.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">05</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/learning-for-planning-epilogue/">Epilogue &mdash; Open Questions</a> <span class="rm-tag">looking forward</span></div>
            <div class="rm-desc">What HOO-POMDP and GammaZero don't yet do, where the next decade of partially-observable planning is heading, and how this all eventually connects to LLM agents, foundation models, and large-scale robotics deployments.</div>
        </div>
    </div>
</div>

<h2>The two paper anchors and how they relate</h2>

<p>This series has two papers, not one. They're not competitors &mdash; they take opposite tacks on the same wall.</p>

<ul>
    <li><strong>HOO-POMDP (Post 3)</strong> &mdash; <em>shrink the problem.</em> Use principled hierarchical abstraction to reduce the effective state space until a classical solver can handle it. Object-oriented belief factorization is the key trick. Works without learning.</li>
    <li><strong>GammaZero (Post 4)</strong> &mdash; <em>keep the problem large; learn to navigate it.</em> Train a GNN on solved small POMDPs; deploy on larger ones. Inherits the GABAR recipe from the sibling series and extends it to belief space.</li>
</ul>

<p>Both posts are <em>also</em> readable standalone as paper deep-dives (under the "My Research" tag), without requiring Posts 1-2. Posts 1-2 are there to anchor the broader context.</p>

<h2>Who this is for</h2>

<div class="audience">
    <div class="aud-card">
        <h4>The LFP series reader</h4>
        <p>You read <a href="/blog/2026/learning-for-planning-overview/">Learning for Planning</a>. This series is the natural next chapter &mdash; same warehouse, same scaling story, now with partial observability. Skim Post 1, dive into Posts 3 and 4.</p>
    </div>
    <div class="aud-card">
        <h4>The POMDP-curious reader</h4>
        <p>You know POMDPs exist but not the modern algorithmic landscape. Read all four posts. Post 1 gives the formalism; Post 2 is the toolkit; Posts 3 and 4 are two ways to attack the heuristic gap that Post 2 reveals.</p>
    </div>
    <div class="aud-card">
        <h4>The robotics reader</h4>
        <p>You operate robots in environments where state is uncertain &mdash; manipulation under occlusion, household tasks, exploration. This series is most directly about your problem. HOO-POMDP (Post 3) is especially relevant for object-rearrangement tasks.</p>
    </div>
</div>

<h2>Reading paths</h2>

<ul>
    <li><strong>Linear (recommended for first read):</strong> 01 &rarr; 02 &rarr; 03 &rarr; 04 &rarr; 05. About 2 hours including time spent on visualizations and side notes.</li>
    <li><strong>Papers only:</strong> Skip to Post 3 (HOO-POMDP) or Post 4 (GammaZero). Each works standalone with a small "Series context" sidebar to orient you.</li>
    <li><strong>Bridge from LFP:</strong> If you came from <a href="/blog/2026/learning-for-planning-epilogue/">LFP's epilogue</a>, start at Post 1 here &mdash; it intentionally references the warehouse you've been reading about. Then jump to Post 4 (GammaZero) for the direct GABAR cousin.</li>
</ul>

<h2>What you won't find here</h2>

<ul>
    <li><strong>No exhaustive POMDP textbook.</strong> Kaelbling, Littman, Cassandra (1998) is the classical reference; Kochenderfer's POMDPs book (2022) is the modern one. This series covers the parts those texts treat as "implementation details."</li>
    <li><strong>No deep-RL for POMDPs.</strong> R2D2, IMPALA, and recurrent policy gradients are real but live in a different ecosystem (offline RL, model-free) than the model-based MCTS framing this series uses.</li>
    <li><strong>No simulator tutorial.</strong> The visualizations are conceptual; the actual experiments in HOO-POMDP and GammaZero use POMDP simulators we won't be teaching from scratch.</li>
</ul>

<hr>

<div class="series-footer">
    <strong>Ready?</strong>
    <p>Start at <a href="/blog/2026/planning-under-uncertainty-belief-states/">Part 1: Planning When You Can't See the Whole World</a>. If you're coming from the sibling series, <a href="/blog/2026/learning-for-planning-epilogue/">LFP's epilogue</a> is the natural bridge. Or jump straight to <a href="/blog/2026/planning-under-uncertainty-gammazero/">GammaZero (Part 4)</a> for the GABAR-shaped paper for belief space.</p>
</div>

</article>
