---
layout: fullhtml-post
title: "Your Belief About the World Is a Graph. Now Your Planner Can Use It."
date: 2026-05-07
categories: ["Planning under Uncertainty", "My Research"]
tags: ["planning", "pomdp", "gnn", "reinforcement-learning"]
description: "How encoding uncertainty as graph structure enables POMDP planners to generalize far beyond their training size. A deep dive into GammaZero. Part 4 of the Planning Under Uncertainty series."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml p { margin: 0 0 1.3em; }
  .blog-fullhtml ul, .blog-fullhtml ol { margin: 0 0 1.3em; }
  .blog-fullhtml h1 { font-size: 2em; line-height: 1.2; margin-top: 1.5em; }
  .blog-fullhtml h2 { font-size: 1.5em; margin-top: 2em; color: #222; }
  .blog-fullhtml h3 { font-size: 1.2em; margin-top: 1.5em; }
  .blog-fullhtml blockquote {
  border-left: 3px solid #5b6abf;
  margin: 1.5em 0;
  padding: 0.5em 1.5em;
  color: #444;
  font-style: italic;
  background: #f8f9ff;
  }
  .blog-fullhtml .subtitle {
  font-size: 1.1em;
  color: #555;
  font-style: italic;
  margin-top: -0.5em;
  }
  .blog-fullhtml hr {
  border: none;
  border-top: 1px solid #ddd;
  margin: 2.5em 0;
  }
  .blog-fullhtml code {
  background: #f4f4f4;
  padding: 2px 5px;
  border-radius: 3px;
  font-size: 0.9em;
  }
  .blog-fullhtml .vis-container {
  margin: 2em 0;
  padding: 1.5em;
  background: #fafafa;
  border-radius: 10px;
  border: 1px solid #eee;
  }
  .blog-fullhtml .vis-caption {
  font-size: 0.85em;
  color: #666;
  font-style: italic;
  margin-top: 10px;
  text-align: center;
  }
  .blog-fullhtml .results-block {
  background: #f0f8ff;
  border-left: 4px solid #5b6abf;
  padding: 15px 20px;
  margin: 1.5em 0;
  border-radius: 0 6px 6px 0;
  }
  .blog-fullhtml .results-block strong { color: #3d4a9e; }
  .blog-fullhtml .equation-note {
  background: #f8f4ff;
  border: 1px dashed #c4b5e0;
  padding: 10px 15px;
  margin: 1em 0;
  border-radius: 4px;
  font-family: monospace;
  font-size: 0.85em;
  color: #4a3580;
  }
  .blog-fullhtml .equation-note::before {
  content: "EQUATION (use Substack's equation button): ";
  font-weight: bold;
  color: #6b5ea0;
  }
  .blog-fullhtml .chart-container {
  margin: 20px 0;
  padding: 20px;
  background: white;
  border-radius: 8px;
  }
  .blog-fullhtml .chart { display: flex; flex-direction: column; gap: 6px; }
  .blog-fullhtml .chart-row { display: flex; align-items: center; gap: 8px; }
  .blog-fullhtml .chart-label { width: 130px; font-size: 0.8em; font-weight: 600; color: #444; text-align: right; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-bar-container { flex: 1; height: 24px; background: #eee; border-radius: 4px; overflow: hidden; }
  .blog-fullhtml .chart-bar { height: 100%; border-radius: 4px; display: flex; align-items: center; justify-content: flex-end; padding-right: 6px; font-size: 0.7em; font-weight: 700; color: white; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-bar.small-text { display: block; padding: 0; line-height: 24px; text-indent: calc(100% + 8px); color: #333; overflow: visible; white-space: nowrap; }
  .blog-fullhtml .chart-title { text-align: center; font-size: 0.9em; font-weight: 700; color: #333; margin-bottom: 12px; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-subtitle { text-align: center; font-size: 0.75em; color: #888; margin-top: -8px; margin-bottom: 12px; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .difficulty-header { font-weight: 700; font-size: 0.8em; color: #555; margin: 10px 0 6px; padding: 3px 8px; background: #f0f0f0; border-radius: 3px; display: inline-block; font-family: -apple-system, sans-serif; }

  .blog-fullhtml .belief-evo { display: flex; gap: 10px; align-items: center; justify-content: center; flex-wrap: wrap; }
  .blog-fullhtml .belief-stage { text-align: center; padding: 12px; background: white; border-radius: 8px; border: 1px solid #eee; min-width: 140px; }
  .blog-fullhtml .belief-stage h5 { font-size: 0.75em; color: #888; text-transform: uppercase; margin-bottom: 6px; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .belief-arrow { font-size: 1.5em; color: #ccc; }

  .blog-fullhtml .compare-table { width: 100%; border-collapse: collapse; font-size: 0.85em; font-family: -apple-system, sans-serif; margin: 15px 0; }
  .blog-fullhtml .compare-table th, .blog-fullhtml .compare-table td { padding: 8px 12px; border: 1px solid #e0e0e0; text-align: center; }
  .blog-fullhtml .compare-table th { background: #f5f5f5; font-weight: 600; }
  .blog-fullhtml .compare-table .yes { color: #2e7d32; font-weight: 600; }
  .blog-fullhtml .compare-table .no { color: #c62828; font-weight: 600; }

  .blog-fullhtml .scale-demo { display: flex; gap: 15px; align-items: flex-end; justify-content: center; margin: 15px 0; }
  .blog-fullhtml .scale-box { text-align: center; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .scale-grid { border: 2px solid #5b6abf; border-radius: 4px; display: inline-block; }
  .blog-fullhtml .scale-label { font-size: 0.7em; color: #666; margin-top: 4px; }
  .blog-fullhtml .scale-arrow { font-size: 1.2em; color: #5b6abf; align-self: center; font-weight: bold; }

  .blog-fullhtml .video-container {
  margin: 2em 0;
  text-align: center;
  }
  .blog-fullhtml .video-placeholder {
  width: 100%;
  max-width: 640px;
  margin: 0 auto;
  aspect-ratio: 16/9;
  background: #f0f0f0;
  border-radius: 10px;
  border: 2px dashed #ccc;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 8px;
  }
  .blog-fullhtml .video-placeholder .play-icon { font-size: 2.5em; color: #bbb; }
  .blog-fullhtml .video-placeholder p { font-size: 0.85em; color: #888; margin: 0; }

  .blog-fullhtml .series-nav {
  background: #ecedfa; border-left: 4px solid #5b6abf; border-radius: 0 8px 8px 0;
  padding: 14px 18px; margin: 0 0 32px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; font-size: 0.92em;
  }
  .blog-fullhtml .series-nav strong { color: #3d4a9e; }
  .blog-fullhtml .series-nav .series-nav-links { margin-top: 6px; font-size: 0.85em; color: #555; }
  .blog-fullhtml .series-nav a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }
  .blog-fullhtml .series-nav a:hover { color: #28327a; border-bottom-color: #28327a; }

  .blog-fullhtml .series-footer {
  margin: 3em 0 2em; padding: 20px 22px;
  background: #f5f6fc; border: 1px solid #d4d8ee; border-radius: 10px;
  font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  .blog-fullhtml .series-footer strong { color: #3d4a9e; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #3d4a9e; text-decoration: none; border-bottom: 1px solid #5b6abf55; }
  .blog-fullhtml .series-footer a:hover { color: #28327a; border-bottom-color: #28327a; }

  .blog-fullhtml .blog-container { max-width: 680px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #b8c8f0; border-bottom-color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml blockquote { background: #1a1f3a; border-left-color: #9aafe6; color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1a1530; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-caption { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .results-block { background: #1a1f3a; border-left-color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .results-block strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .equation-note { background: #1e142e; border-color: #6b3fa0; color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .equation-note::before { color: #b088e0; }
  html[data-theme="dark"] .blog-fullhtml .chart-container { background: #1a2030; }
  html[data-theme="dark"] .blog-fullhtml .chart-label { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .chart-bar-container { background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .chart-bar.small-text { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .chart-title { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .chart-subtitle { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .difficulty-header { color: #a8b8b8; background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .belief-stage { background: #1a1530; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .belief-stage h5 { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .belief-arrow { color: #5a6065; }
  html[data-theme="dark"] .blog-fullhtml .compare-table th, html[data-theme="dark"] .blog-fullhtml .compare-table td { border-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .compare-table th { background: #1e2530; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .yes { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .no { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .partial { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .scale-grid { border-color: #5b6abf; }
  html[data-theme="dark"] .blog-fullhtml .scale-label { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .scale-arrow { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .video-placeholder { background: #1a1530; border-color: #3a3545; }
  html[data-theme="dark"] .blog-fullhtml .video-placeholder .play-icon { color: #5a5560; }
  html[data-theme="dark"] .blog-fullhtml .video-placeholder p { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #1a1f3a; border-left-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #b8c8f0; border-bottom-color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #15192e; border-color: #3a4a6a; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #9aafe6; border-bottom-color: rgba(154,175,230,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-footer a:hover { color: #b8c8f0; border-bottom-color: #b8c8f0; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<article class="blog-container">

<!-- Series Nav (Planning Under Uncertainty · Part 4 of 4) -->
<div class="series-nav">
    <strong>Planning Under Uncertainty · Part 4 of 4 — the finale</strong>
    <div class="series-nav-links">
        ← <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">Part 3: HOO-POMDP</a> · Next: <a href="/blog/2026/planning-under-uncertainty-epilogue/">Epilogue: Open Questions →</a> · This is the series anchor — a deep dive on the GammaZero paper. <em>(Also readable standalone.)</em>
    </div>
</div>

<h1>Your Belief About the World Is a Graph. Now Your Planner Can Use It.</h1>

<p class="subtitle">How encoding uncertainty as graph structure enables POMDP planners to generalize far beyond their training size. A deep dive into GammaZero.</p>

<p style="background:#f5f6fc; border-left:3px solid #5b6abf; padding:10px 14px; font-size:0.92em; color:#3a4475; margin-top:14px; border-radius:0 6px 6px 0;">
    <strong>Also part of a series.</strong> This paper deep-dive stands on its own — read it as one. If you want more context: it is the finale of the <em>Planning Under Uncertainty</em> series. Where <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/" style="color:#3d4a9e;">Part 3 (HOO-POMDP)</a> took the <em>abstraction</em> strategy, GammaZero takes the <em>learning</em> strategy. It is also the partially-observable cousin of <a href="/blog/2026/learning-for-planning-gabar/" style="color:#3d4a9e;">GABAR</a> from the sibling <em>Learning for Planning</em> series: same idea, now extended to belief states.
</p>

<p style="background:#FDF6E3; border-left:3px solid #C5A55A; padding:10px 14px; font-size:0.9em; color:#5a4400; margin-top:10px; border-radius:0 6px 6px 0;">
    <strong>Running example:</strong> The series uses warehouse-delivery as its running example end-to-end &mdash; robot, packages, zones, with the robot only seeing its current zone. GammaZero's experiments use <em>RockSample</em>, <em>MultiObjectSearch</em>, and other relational POMDPs, but the core mental model is the same warehouse: take an unknown configuration, build a belief graph, score actions, act, observe, repeat. The <a href="/blog/2026/learning-for-planning-gabar/" style="color:#3d4a9e;">GABAR exec-loop visualization</a> from the LFP series shows the structure; GammaZero's version operates over a belief graph instead.
</p>

<hr>

<h2>The Setup: Planning When You Can't See Everything</h2>

<p>Imagine you're a Mars rover. You can see the terrain directly in front of you, but you can't see what's behind the next ridge. You have a noisy sensor that gives you partial readings about whether a rock formation is scientifically valuable. You need to decide: do you spend time scanning that distant rock (which might be worthless), or do you move toward the exit to meet your deadline?</p>

<p>This is a <strong>Partially Observable Markov Decision Process</strong> (POMDP). Unlike fully observable problems where you know exactly what state you're in, here you maintain a <em>belief</em>—a probability distribution over possible states. Every action you take both changes the world and updates your belief about it.</p>

<p>The challenge is severe. Even for small problems, solving POMDPs exactly is computationally intractable. The gold-standard approach is <strong>Monte Carlo Tree Search (MCTS)</strong>—build a search tree by simulating possible futures. But MCTS struggles with long planning horizons. Without good heuristics to guide the search, it can't look far enough ahead to find rewarding action sequences that require extended information gathering.</p>

<p>Recent work like <strong>BetaZero</strong> has shown that neural networks can learn to guide MCTS effectively, replacing hand-crafted heuristics with learned value functions and policies. The approach works: train a network on expert demonstrations, then use it to prioritize actions and estimate values during search.</p>

<p>But there's a catch.</p>

<hr>

<h2>The Representation Bottleneck</h2>

<p>BetaZero and similar approaches represent belief states as fixed-dimensional vectors—statistical summaries of the particle set. This creates a rigid architecture that must be redesigned for each problem size.</p>

<p>Consider a robot searching for objects on a grid:</p>

<div class="vis-container">
    <div class="scale-demo">
        <div class="scale-box">
            <div class="scale-grid" style="width:50px;height:50px;background:linear-gradient(135deg,#e3f2fd 25%,#bbdefb 75%);"></div>
            <div class="scale-label">5x5 grid<br>3 objects<br><strong>Train here</strong></div>
        </div>
        <div class="scale-arrow">&#8594;</div>
        <div class="scale-box">
            <div class="scale-grid" style="width:80px;height:80px;background:linear-gradient(135deg,#e3f2fd 25%,#bbdefb 75%);"></div>
            <div class="scale-label">10x10 grid<br>10 objects<br><em>Can't deploy</em></div>
        </div>
        <div class="scale-arrow">&#8594;</div>
        <div class="scale-box">
            <div class="scale-grid" style="width:110px;height:110px;background:linear-gradient(135deg,#e3f2fd 25%,#bbdefb 75%);"></div>
            <div class="scale-label">20x20 grid<br>20 objects<br><em>Retrain from scratch</em></div>
        </div>
    </div>
    <p class="vis-caption">BetaZero's fixed-dimensional representation requires retraining for each problem size. The network architecture itself encodes the problem dimensions.</p>
</div>

<p>A network trained on a 5x5 grid with 3 objects literally <em>cannot process</em> a 10x10 grid with 10 objects—the input dimensions don't match. You need to retrain from scratch, which requires running expensive expert planners on the larger problems to generate training data. But those expert planners are exactly what you're trying to avoid using.</p>

<p>This creates a circular dependency: you need the planner to train the network, but you need the network to make the planner tractable.</p>

<blockquote>What if the representation itself could grow with the problem, while the patterns learned on small instances still applied?</blockquote>

<hr>

<h2>The lineage: how GammaZero got here</h2>

<p>GammaZero is the convergence point of three lines of work. To see why each design choice was forced rather than arbitrary, it helps to walk the lineage.</p>

<h3>AlphaZero &mdash; learning to guide MCTS in fully observable games</h3>

<p><strong>AlphaZero</strong> (Silver et al., 2018) demonstrated that learned value and policy heads can replace the hand-crafted heuristics inside Monte Carlo tree search. The architecture is a network <code>f_&theta;(s) = (V_&theta;(s), P_&theta;(a|s))</code> that takes a state and predicts both its value and a prior distribution over actions. During search, the PUCT selection rule weights the standard exploration term by the learned prior, so simulations concentrate on actions the policy judges promising. At leaf nodes, the value prediction replaces the rollout return directly &mdash; one forward pass instead of a noisy average over many random trajectories.</p>

<p>The training procedure is self-play. The network guides MCTS, the search produces an improved policy (the visit counts at the root), and the network is updated toward that improved policy and toward the observed returns. The improved policy then guides the next round of search. AlphaZero's success in Go, chess, and shogi without any hand-crafted heuristics established this as the dominant recipe for MCTS-driven game playing.</p>

<p><strong>MuZero</strong> (Schrittwieser et al., 2020) later extended the template to settings where the transition model is not known, by jointly learning a latent dynamics model alongside the value and policy heads. This made AlphaZero-style learning applicable to Atari and other domains where ground-truth simulators aren't available.</p>

<p>Both AlphaZero and MuZero live in the fully observable setting. The state is observable; the network input is a fixed-shape board (or a fixed-shape frame). The recipe doesn't transfer to POMDPs out of the box because the belief space is high-dimensional and continuous &mdash; there is no fixed-shape input to feed the network.</p>

<h3>BetaZero &mdash; AlphaZero for POMDPs, with fixed-dimensional beliefs</h3>

<p><strong>BetaZero</strong> (Moss et al., 2024) is the current state-of-the-art instantiation of the AlphaZero template for POMDPs and the most direct prior work for GammaZero. It trains neural networks that predict values and action probabilities from belief states, and uses those networks to guide MCTS at decision time. Training proceeds AlphaZero-style: run the planner on small instances, use the search to produce improved policies, update the network toward those policies and toward the observed returns. The benchmark results are strong &mdash; on LightDark, RockSample, and similar problems, BetaZero matches or exceeds classical online solvers while dramatically reducing per-decision cost.</p>

<p>The catch is the belief representation. BetaZero summarizes the particle-based belief into a <em>fixed-dimensional feature vector</em>: per-state probability estimates, attribute-wise marginals, or hand-engineered summary features. The dimensionality is chosen once per problem size. A network trained on RockSample with an 11&times;11 grid and 11 rocks cannot be applied to a 15&times;15 grid with 15 rocks, because the belief vector has a different length and the network's first-layer weights are tied to that length. <em>The very problems where learning could help most &mdash; large instances where classical planners struggle &mdash; are exactly the ones that fixed-dimensional models trained on small instances cannot handle.</em></p>

<p><strong>ConstrainedZero</strong> (Moss et al., 2025) extends BetaZero to chance-constrained POMDPs where the agent must satisfy safety constraints with high probability. It adds heads that predict constraint satisfaction and modifies the search to prune actions whose estimated constraint satisfaction falls below a threshold. <strong>LeTS-Drive</strong> (Cai et al., 2022) applies the learning-guided search template to autonomous driving, training the belief estimator and planning network jointly. Both extensions preserve BetaZero's fixed-dimensional belief encoder; both inherit its size-generalization limitation.</p>

<h3>GABAR &mdash; the fully-observable cousin</h3>

<p>If BetaZero is the obvious POMDP precursor, <a href="/blog/2026/learning-for-planning-gabar/">GABAR</a> from the sibling series is the obvious representational precursor. GABAR demonstrated that a graph neural network over a relational planning state generalizes across instance sizes &mdash; the same trained network solves instances 8&times; larger than anything it trained on, without retraining. The graph topology grows with the problem; the network's weights are shared across nodes; size generalization comes for free.</p>

<p>GABAR is fully observable. The mapping from state to graph is immediate: each ground atom either holds or does not. There is no notion of probability over nodes. To carry the recipe into the partially observable setting, the graph construction itself has to change to encode uncertainty.</p>

<h3>The convergence</h3>

<p>GammaZero sits at the intersection. From AlphaZero/MuZero: the value+policy network template and the PUCT selection rule that exploits it. From BetaZero: the application to POMDPs and the particle-based belief substrate. From GABAR: the graph-based representation that handles variable-sized inputs through GNN message passing. The contribution &mdash; what no prior method had done &mdash; is the construction that encodes <em>belief uncertainty in the graph topology itself</em>, so the same network can guide MCTS on POMDPs of any size.</p>

<div class="vis-container">
    <h3 style="font-family:'Playfair Display',Georgia,serif; font-size:1.2rem; color:#2D2044; font-weight:700; margin:0;">Same foggy warehouse, two belief encodings</h3>
    <p style="color:#888; font-size:0.92em; margin-top:6px; margin-bottom:14px; font-style:italic;">BetaZero's fixed-dim vector vs GammaZero's variable-size graph &mdash; on the same partially observable warehouse.</p>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0; border:1.5px solid #D4CDE0; border-radius:9px; padding:10px; background:#fff;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#C0392B; margin-bottom:6px;">BetaZero · fixed-dimensional belief vector</div>
            <svg viewBox="0 0 280 220" preserveAspectRatio="xMidYMid meet" style="width:100%; height:auto;">
                <!-- Foggy warehouse mini -->
                <defs>
                    <radialGradient id="fogLB" cx="50%" cy="50%" r="55%"><stop offset="0%" stop-color="#2E1A38" stop-opacity=".25"/><stop offset="100%" stop-color="#2E1A38" stop-opacity=".55"/></radialGradient>
                </defs>
                <rect x="10" y="6" width="80" height="36" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <text x="22" y="22" fill="#B0A8C0" font-size="8" font-weight="600">A</text>
                <text x="60" y="22" fill="#B0A8C0" font-size="8" font-weight="600">B</text>
                <text x="22" y="38" fill="#B0A8C0" font-size="8" font-weight="600">C</text>
                <text x="60" y="38" fill="#B0A8C0" font-size="8" font-weight="600">D</text>
                <rect x="30" y="6" width="60" height="36" fill="url(#fogLB)"/>
                <text x="50" y="22" text-anchor="middle" fill="#E6E0EC" font-size="9" font-weight="700" opacity=".6">?</text>
                <text x="50" y="38" text-anchor="middle" fill="#E6E0EC" font-size="9" font-weight="700" opacity=".6">?</text>

                <!-- Arrow -->
                <text x="50" y="58" text-anchor="middle" fill="#6B5B7B" font-size="9" font-style="italic">flatten to vector</text>
                <line x1="50" y1="62" x2="50" y2="76" stroke="#C0392B" stroke-width="1.5" marker-end="url(#arrDn1)"/>
                <defs><marker id="arrDn1" markerWidth="6" markerHeight="6" refX="3" refY="5" orient="auto"><path d="M0,0 L3,5 L6,0" fill="#C0392B"/></marker></defs>

                <!-- Fixed-dim vector -->
                <rect x="0" y="84" width="100" height="32" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <text x="50" y="98" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">⟨0.34, 0.21, 0.12, ...⟩</text>
                <text x="50" y="111" text-anchor="middle" fill="#C0392B" font-size="8" font-style="italic">12-dim feature vector</text>

                <!-- Trained network -->
                <rect x="10" y="128" width="80" height="32" rx="4" fill="#E8E4ED" stroke="#5b6abf" stroke-width="1.2"/>
                <text x="50" y="142" text-anchor="middle" fill="#3d4a9e" font-size="9" font-weight="700">trained net (12 → 1)</text>
                <text x="50" y="154" text-anchor="middle" fill="#3d4a9e" font-size="8" font-style="italic">V_θ, P_θ outputs</text>

                <!-- The breaking point -->
                <line x1="100" y1="100" x2="160" y2="100" stroke="#888" stroke-width="1.5" stroke-dasharray="3 3" marker-end="url(#arrR1)"/>
                <defs><marker id="arrR1" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto"><path d="M0,0 L5,3 L0,6" fill="#888"/></marker></defs>

                <!-- 16-zone large variant -->
                <rect x="170" y="80" width="100" height="40" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <g>
                <rect x="172" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="194" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="216" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="238" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="172" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="194" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="216" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="238" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="172" y="82" width="86" height="37" fill="url(#fogLB)"/>
                </g>
                <text x="220" y="100" text-anchor="middle" fill="#E6E0EC" font-size="10" font-weight="700" opacity=".7">larger</text>

                <!-- Failure indicator -->
                <line x1="220" y1="120" x2="220" y2="140" stroke="#C0392B" stroke-width="2"/>
                <rect x="155" y="142" width="130" height="60" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <text x="220" y="158" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">✗ Can't apply trained net</text>
                <text x="220" y="172" text-anchor="middle" fill="#5a2a23" font-size="8.5">Belief vector now 24-dim,</text>
                <text x="220" y="184" text-anchor="middle" fill="#5a2a23" font-size="8.5">network input layer was 12.</text>
                <text x="220" y="197" text-anchor="middle" fill="#5a2a23" font-size="8.5" font-style="italic">Must retrain from scratch.</text>
            </svg>
        </div>
        <div style="flex:1; min-width:0; border:1.5px solid rgba(91,106,191,.4); border-radius:9px; padding:10px; background:#fafcff;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#3d4a9e; margin-bottom:6px;">GammaZero · belief graph (variable size)</div>
            <svg viewBox="0 0 280 220" preserveAspectRatio="xMidYMid meet" style="width:100%; height:auto;">
                <defs>
                    <radialGradient id="fogLG" cx="50%" cy="50%" r="55%"><stop offset="0%" stop-color="#2E1A38" stop-opacity=".25"/><stop offset="100%" stop-color="#2E1A38" stop-opacity=".55"/></radialGradient>
                </defs>

                <!-- Foggy warehouse mini -->
                <rect x="10" y="6" width="80" height="36" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <text x="22" y="22" fill="#B0A8C0" font-size="8" font-weight="600">A</text>
                <text x="60" y="22" fill="#B0A8C0" font-size="8" font-weight="600">B</text>
                <text x="22" y="38" fill="#B0A8C0" font-size="8" font-weight="600">C</text>
                <text x="60" y="38" fill="#B0A8C0" font-size="8" font-weight="600">D</text>
                <rect x="30" y="6" width="60" height="36" fill="url(#fogLG)"/>

                <text x="50" y="58" text-anchor="middle" fill="#3d4a9e" font-size="9" font-style="italic">construct belief graph</text>
                <line x1="50" y1="62" x2="50" y2="76" stroke="#5b6abf" stroke-width="1.5" marker-end="url(#arrDn2)"/>
                <defs><marker id="arrDn2" markerWidth="6" markerHeight="6" refX="3" refY="5" orient="auto"><path d="M0,0 L3,5 L6,0" fill="#5b6abf"/></marker></defs>

                <!-- Small belief graph: 5 nodes -->
                <g transform="translate(0,84)">
                    <circle cx="20" cy="20" r="10" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                    <text x="20" y="23" text-anchor="middle" fill="#B08E2A" font-size="7" font-weight="700">R</text>
                    <circle cx="60" cy="10" r="10" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                    <text x="60" y="13" text-anchor="middle" fill="#B08E2A" font-size="7" font-weight="700">P</text>
                    <circle cx="90" cy="30" r="10" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1.2"/>
                    <text x="90" y="33" text-anchor="middle" fill="#B08E2A" font-size="7" font-weight="700">A</text>
                    <line x1="20" y1="20" x2="60" y2="10" stroke="#C0392B" stroke-width="1.4" stroke-opacity=".55"/>
                    <line x1="20" y1="20" x2="90" y2="30" stroke="#C0392B" stroke-width="0.8" stroke-opacity=".25"/>
                    <line x1="60" y1="10" x2="90" y2="30" stroke="#C0392B" stroke-width="0.4" stroke-opacity=".15"/>
                </g>

                <!-- GNN network -->
                <rect x="10" y="128" width="80" height="32" rx="4" fill="#E8E4ED" stroke="#5b6abf" stroke-width="1.2"/>
                <text x="50" y="142" text-anchor="middle" fill="#3d4a9e" font-size="9" font-weight="700">GNN (V_θ, P_θ)</text>
                <text x="50" y="154" text-anchor="middle" fill="#3d4a9e" font-size="8" font-style="italic">same weights, any size</text>

                <!-- Arrow -->
                <line x1="100" y1="100" x2="160" y2="100" stroke="#5b6abf" stroke-width="1.5" marker-end="url(#arrR2)"/>
                <defs><marker id="arrR2" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto"><path d="M0,0 L5,3 L0,6" fill="#5b6abf"/></marker></defs>

                <!-- 16-zone large variant -->
                <rect x="170" y="80" width="100" height="40" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <g>
                <rect x="172" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="194" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="216" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="238" y="82" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="172" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="194" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="216" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="238" y="101" width="20" height="18" rx="2" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".3"/>
                <rect x="172" y="82" width="86" height="37" fill="url(#fogLG)"/>
                </g>

                <!-- Bigger belief graph at right -->
                <g transform="translate(165,135)">
                    <circle cx="15" cy="15" r="6" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                    <text x="15" y="17" text-anchor="middle" fill="#B08E2A" font-size="5" font-weight="700">R</text>
                    <circle cx="45" cy="8" r="6" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                    <circle cx="75" cy="20" r="6" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                    <circle cx="105" cy="8" r="6" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                    <circle cx="45" cy="38" r="6" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                    <circle cx="75" cy="50" r="6" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                    <circle cx="105" cy="38" r="6" fill="#FDF6E3" stroke="#B08E2A" stroke-width="1"/>
                    <line x1="15" y1="15" x2="45" y2="8" stroke="#C0392B" stroke-width="1.2" stroke-opacity=".55"/>
                    <line x1="15" y1="15" x2="45" y2="38" stroke="#C0392B" stroke-width="0.4" stroke-opacity=".25"/>
                    <line x1="45" y1="8" x2="75" y2="20" stroke="#C0392B" stroke-width="0.8" stroke-opacity=".35"/>
                    <line x1="75" y1="20" x2="105" y2="8" stroke="#C0392B" stroke-width="0.6" stroke-opacity=".3"/>
                    <line x1="75" y1="20" x2="105" y2="38" stroke="#C0392B" stroke-width="1.0" stroke-opacity=".4"/>
                    <line x1="45" y1="38" x2="75" y2="50" stroke="#C0392B" stroke-width="0.7" stroke-opacity=".3"/>
                </g>

                <!-- Success indicator -->
                <rect x="155" y="195" width="130" height="22" rx="4" fill="#E8F4F0" stroke="#1E8449" stroke-width="1.4"/>
                <text x="220" y="209" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700">✓ Same network, larger graph</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #ecedfa; border-radius: 8px; border-left: 4px solid #5b6abf;">
        <p style="font-size: 0.92em; color: #3a4475; line-height: 1.5; margin: 0;">BetaZero's network input is a fixed-shape vector summarizing the belief. When the warehouse grows, the vector grows, and the network's first-layer weights no longer match. GammaZero replaces the vector with a graph &mdash; the graph topology grows naturally with the warehouse, but the GNN's weight set is shared across nodes, so the same trained network handles both sizes.</p>
    </div>
</div>

<hr>

<h2>The Core Insight: Belief States Are Graphs</h2>

<p>Here's the key idea behind GammaZero: a belief state—that probability distribution over possible worlds—has <em>relational structure</em>. Objects have attributes. Actions connect to objects. Uncertainty creates specific patterns of connectivity between what you know and what you don't.</p>

<p>Instead of flattening this structure into a fixed vector, we preserve it as a graph:</p>

<div class="vis-container">
    <div class="belief-evo">
        <div class="belief-stage">
            <h5>Particle Belief</h5>
            <svg width="100" height="80" viewBox="0 0 100 80">
                <circle cx="30" cy="25" r="3" fill="#42a5f5" opacity="0.6"/>
                <circle cx="50" cy="30" r="3" fill="#42a5f5" opacity="0.6"/>
                <circle cx="70" cy="20" r="3" fill="#42a5f5" opacity="0.6"/>
                <circle cx="25" cy="50" r="3" fill="#42a5f5" opacity="0.6"/>
                <circle cx="55" cy="55" r="3" fill="#42a5f5" opacity="0.6"/>
                <circle cx="75" cy="45" r="3" fill="#42a5f5" opacity="0.6"/>
                <circle cx="40" cy="65" r="3" fill="#42a5f5" opacity="0.6"/>
                <circle cx="60" cy="70" r="3" fill="#42a5f5" opacity="0.6"/>
                <text x="50" y="12" text-anchor="middle" font-size="7" fill="#888">N particles</text>
            </svg>
        </div>
        <div class="belief-arrow">&#8594;</div>
        <div class="belief-stage">
            <h5>Aggregate</h5>
            <svg width="100" height="80" viewBox="0 0 100 80">
                <rect x="10" y="15" width="80" height="14" rx="3" fill="#eee" stroke="#ccc"/>
                <rect x="10" y="15" width="56" height="14" rx="3" fill="#81c784"/>
                <text x="50" y="25" text-anchor="middle" font-size="6.5" fill="#333">IsGood: 0.75</text>
                <rect x="10" y="38" width="80" height="14" rx="3" fill="#eee" stroke="#ccc"/>
                <rect x="10" y="38" width="32" height="14" rx="3" fill="#9e9e9e"/>
                <text x="50" y="48" text-anchor="middle" font-size="6.5" fill="#333">IsGood: 0.40</text>
                <rect x="10" y="61" width="80" height="14" rx="3" fill="#eee" stroke="#ccc"/>
                <rect x="10" y="61" width="72" height="14" rx="3" fill="#4caf50"/>
                <text x="50" y="71" text-anchor="middle" font-size="6.5" fill="white">At(bot): 1.00</text>
            </svg>
        </div>
        <div class="belief-arrow">&#8594;</div>
        <div class="belief-stage">
            <h5>Belief Graph</h5>
            <svg width="110" height="80" viewBox="0 0 110 80">
                <line x1="30" y1="20" x2="55" y2="45" stroke="#5b6abf" stroke-width="1.5"/>
                <line x1="80" y1="20" x2="55" y2="45" stroke="#5b6abf" stroke-width="1.5"/>
                <line x1="55" y1="45" x2="35" y2="70" stroke="#ff9800" stroke-width="1.2" stroke-dasharray="3,2"/>
                <line x1="55" y1="45" x2="75" y2="70" stroke="#ff9800" stroke-width="1.5" stroke-dasharray="3,2"/>
                <rect x="15" y="10" width="30" height="16" rx="3" fill="#5b6abf"/>
                <text x="30" y="21" text-anchor="middle" font-size="5.5" fill="white">Check</text>
                <rect x="65" y="10" width="30" height="16" rx="3" fill="#5b6abf"/>
                <text x="80" y="21" text-anchor="middle" font-size="5.5" fill="white">Sample</text>
                <circle cx="55" cy="45" r="9" fill="#c8e6c9" stroke="#388e3c" stroke-width="1.5"/>
                <text x="55" y="48" text-anchor="middle" font-size="6" fill="#2e7d32">R2</text>
                <rect x="20" y="63" width="30" height="12" rx="6" fill="#ff9800" opacity="0.5"/>
                <text x="35" y="72" text-anchor="middle" font-size="5" fill="white">0.40</text>
                <rect x="60" y="63" width="30" height="12" rx="6" fill="#ff9800" opacity="0.85"/>
                <text x="75" y="72" text-anchor="middle" font-size="5" fill="white">0.75</text>
            </svg>
        </div>
    </div>
    <p class="vis-caption">GammaZero's belief-to-graph pipeline. Particles are aggregated into attribute probabilities, then selectively instantiated as graph nodes based on threshold tau. The resulting graph encodes both structure and uncertainty.</p>
</div>

<!-- TODO: re-enable once the demo recording is ready
<div class="video-container">
    <p style="font-size:0.9em;color:#444;font-weight:600;margin-bottom:8px;">Interactive Graph Construction Demo</p>
    <div class="video-placeholder">
        <div class="play-icon">&#9654;</div>
        <p>Video: Belief-to-Graph Construction Walkthrough</p>
        <p style="font-size:0.75em;">[Upload your screen recording of the interactive demo here]</p>
    </div>
    <p style="font-size:0.78em;color:#888;margin-top:8px;">Watch how hovering over belief state elements reveals the corresponding graph nodes and edges. Each rock's uncertainty level determines which attribute nodes exist and their connection strengths.</p>
</div>
-->


<p>The graph has four types of nodes:</p>

<ul>
    <li><strong>Object nodes</strong> — entities in the environment (robot, rocks, locations)</li>
    <li><strong>Attribute nodes</strong> — properties that hold with sufficient probability (e.g., "IsGood(Rock2) = 0.75")</li>
    <li><strong>Action nodes</strong> — available actions connected to their parameter objects</li>
    <li><strong>A global node</strong> — aggregates information across the entire graph</li>
</ul>

<p>The critical innovation: <strong>attribute nodes are only created when belief probability exceeds a threshold tau</strong>. If you're 75% sure a rock is good, that belief becomes a node. If you're only 10% sure, it doesn't. This means the graph structure itself encodes what you believe—not just numerical features on nodes, but which nodes <em>exist</em>.</p>

<p>Adding more objects to the problem simply adds more nodes and edges to the graph. The same Graph Neural Network (GNN) processes graphs of any size using identical weights. Patterns learned on small graphs transfer directly to larger ones.</p>

<hr>

<h2>Why This Representation Works: The Three Properties</h2>

<p>Not every graph representation would enable generalization. GammaZero's works because of three specific properties:</p>

<h3>1. Structural Encoding of Uncertainty</h3>

<p>Most approaches encode uncertainty as numbers—a 15-dimensional vector of belief statistics. GammaZero encodes it <em>structurally</em>. When a rock's quality is uncertain, both "IsGood(R1) = 0.4" and "IsBad(R1) = 0.6" exist as separate nodes. When you're certain, only one exists. The GNN learns to recognize these structural patterns:</p>

<ul>
    <li>Two complementary attribute nodes with similar weights → high uncertainty → information gathering is valuable</li>
    <li>A single attribute node with high weight → low uncertainty → commitment is safe</li>
</ul>

<p>These patterns are <em>size-invariant</em>. The same uncertainty signature around Rock 1 in a 5x5 grid means the same thing around Rock 15 in a 25x25 grid.</p>

<h3>2. Action-Centric Connectivity</h3>

<p>Each action node connects to exactly the objects it operates on. Check(R1) connects to R1. Sample(R3) connects to R3. MoveEast connects to the robot. This makes action evaluation a <em>local computation</em>—the GNN can assess action quality by examining the neighborhood of each action node.</p>

<p>The attribute-action edges are especially powerful: they directly encode which attributes are relevant to which actions. If IsGood(R3) has high belief and is connected to Sample(R3), the network learns this is a good sampling opportunity—regardless of how many other rocks exist in the problem.</p>

<h3>3. Global Context Through Aggregation</h3>

<p>The global node connects to all object nodes and maintains a holistic representation of the belief state. This provides "shortcut" information propagation: even in a large graph where two nodes might be far apart, they both communicate through the global node at every message-passing round.</p>

<p>This is crucial for decisions that require global context—like deciding whether to gather more information or head for the exit. The local neighborhood of "MoveEast" shows the robot's position, but the global node aggregates how many high-value rocks remain unsampled across the entire grid.</p>

<hr>

<h2>The Full System: From Graphs to Decisions</h2>

<p>GammaZero operates in two phases:</p>

<h3>Offline: Learn from Small Problems</h3>

<ol>
    <li>Run optimal planners on small POMDP instances (5x5 grids, 3-5 objects)</li>
    <li>Collect belief states encountered during planning, along with optimal actions and values</li>
    <li>Convert each belief to a graph using the construction above</li>
    <li>Train a GNN to predict both values V(G) and action distributions P(a|G)</li>
</ol>

<p>Training takes 2-4 hours on a single GPU. The training data is essentially free—small problems are solved in milliseconds by existing planners.</p>

<h3>Online: Guide Search on Large Problems</h3>

<p>During deployment on larger problems (15x15 grid, 15 rocks), the learned GNN enhances MCTS in three ways:</p>

<div class="results-block">
<p><strong>Action Prioritization:</strong> Instead of exploring actions uniformly, MCTS samples from P(a|G), focusing search on promising actions first.</p>
<p><strong>Value Estimation:</strong> Instead of expensive rollouts to estimate leaf values, MCTS uses V(G) for instant evaluation.</p>
<p><strong>Action Selection:</strong> The final action combines visit counts with learned Q-values for robust selection.</p>
</div>

<p>The GNN processes the larger graph using the exact same weights trained on small instances. No retraining, no architecture changes, no new expert data needed.</p>

<hr>

<h2>Results: Generalization That Actually Works</h2>

<p>The experiments answer two questions: Can GammaZero match existing methods on same-sized problems? And can it generalize to larger ones?</p>

<h3>Same-Size Performance</h3>

<p>When trained and tested on identical problem sizes, GammaZero matches or exceeds BetaZero across all domains:</p>

<div class="vis-container">
    <div class="chart-container">
        <div class="chart-title">Same-Size Performance (Average Return)</div>
        <div class="chart-subtitle">Higher is better. All methods trained and tested on same problem size.</div>
        <div class="chart">
            <div class="difficulty-header">LightDark(10)</div>
            <div class="chart-row"><div class="chart-label">GammaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:87.5%;background:#5b6abf;">17.5</div></div></div>
            <div class="chart-row"><div class="chart-label">BetaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:83.8%;background:#8e99cc;">16.8</div></div></div>
            <div class="chart-row"><div class="chart-label">AdaOPS</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:26.1%;background:#bbb;">5.2</div></div></div>

            <div class="difficulty-header" style="margin-top:12px">RockSample(15,15)</div>
            <div class="chart-row"><div class="chart-label">GammaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:82%;background:#5b6abf;">20.5</div></div></div>
            <div class="chart-row"><div class="chart-label">BetaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:80.6%;background:#8e99cc;">20.2</div></div></div>
            <div class="chart-row"><div class="chart-label">AdaOPS</div><div class="chart-bar-container"><div class="chart-bar" style="width:82.7%;background:#bbb;">20.7</div></div></div>

            <div class="difficulty-header" style="margin-top:12px">MultiObjectSearch(5,3)</div>
            <div class="chart-row"><div class="chart-label">GammaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:90%;background:#5b6abf;">18.0</div></div></div>
            <div class="chart-row"><div class="chart-label">AdaOPS</div><div class="chart-bar-container"><div class="chart-bar" style="width:77.5%;background:#bbb;">15.5</div></div></div>
            <div class="chart-row"><div class="chart-label">POMCPOW</div><div class="chart-bar-container"><div class="chart-bar" style="width:37.5%;background:#ddd;">7.5</div></div></div>
        </div>
    </div>
    <p class="vis-caption">GammaZero consistently matches or outperforms BetaZero on same-sized problems, while substantially outperforming classical baselines in domains requiring information gathering.</p>
</div>

<p>This validates that the graph representation doesn't sacrifice performance—it captures everything the fixed-dimensional approach captures, and more.</p>

<h3>Zero-Shot Generalization</h3>

<p>The unique capability: GammaZero can generalize to problems significantly larger than training instances. BetaZero cannot—its architecture physically won't accept larger inputs.</p>

<div class="vis-container">
    <div class="chart-container">
        <div class="chart-title">Zero-Shot Generalization (Average Return)</div>
        <div class="chart-subtitle">GammaZero trained on small instances (5x5 to 10x10), tested on larger. Classical baselines retrained per-size.</div>
        <div class="chart">
            <div class="difficulty-header">RockSample(15,15) — 2.25x training area</div>
            <div class="chart-row"><div class="chart-label">GammaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:85%;background:#5b6abf;">17.8</div></div></div>
            <div class="chart-row"><div class="chart-label">DESPOT</div><div class="chart-bar-container"><div class="chart-bar" style="width:88%;background:#bbb;">18.4</div></div></div>
            <div class="chart-row"><div class="chart-label">POMCPOW</div><div class="chart-bar-container"><div class="chart-bar" style="width:53%;background:#ddd;">11.1</div></div></div>

            <div class="difficulty-header" style="margin-top:12px">RockSample(20,20) — 4x training area</div>
            <div class="chart-row"><div class="chart-label">GammaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:75%;background:#5b6abf;">10.2</div></div></div>
            <div class="chart-row"><div class="chart-label">AdaOPS</div><div class="chart-bar-container"><div class="chart-bar" style="width:85%;background:#bbb;">11.7</div></div></div>
            <div class="chart-row"><div class="chart-label">DESPOT</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:5%;background:#e74c3c;">timeout</div></div></div>

            <div class="difficulty-header" style="margin-top:12px">RockSample(25,25) — 6.25x training area</div>
            <div class="chart-row"><div class="chart-label">GammaZero (P only)</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:48%;background:#7c8ad4;">4.8</div></div></div>
            <div class="chart-row"><div class="chart-label">AdaOPS</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:42%;background:#bbb;">4.2</div></div></div>
            <div class="chart-row"><div class="chart-label">DESPOT</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:5%;background:#e74c3c;">timeout</div></div></div>

            <div class="difficulty-header" style="margin-top:12px">MOS(8,6) — 4x objects, 4x area vs training</div>
            <div class="chart-row"><div class="chart-label">GammaZero</div><div class="chart-bar-container"><div class="chart-bar" style="width:80%;background:#5b6abf;">8.0</div></div></div>
            <div class="chart-row"><div class="chart-label">AdaOPS</div><div class="chart-bar-container"><div class="chart-bar" style="width:58%;background:#bbb;">5.8</div></div></div>
            <div class="chart-row"><div class="chart-label">POMCPOW/DESPOT</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width:5%;background:#e74c3c;">timeout</div></div></div>
        </div>
    </div>
    <p class="vis-caption">GammaZero trained on small instances generalizes to problems 2-6x larger. Classical baselines increasingly fail with timeouts at larger scales.</p>
</div>

<p>Key observations:</p>

<ul>
    <li><strong>Graceful degradation:</strong> Performance drops gradually as problem size increases, rather than catastrophically failing</li>
    <li><strong>Competitive without retraining:</strong> On RockSample(20,20), GammaZero (trained only on small instances) achieves 87% of AdaOPS's performance (which was specifically configured for that problem size)</li>
    <li><strong>Classical planners fail:</strong> At RockSample(25,25) and MOS(8,6), DESPOT and POMCPOW simply timeout, while GammaZero still produces reasonable solutions</li>
    <li><strong>Raw policy suffices at extreme scales:</strong> On RS(25,25), the raw learned policy (without MCTS) achieves the best result, suggesting the learned structural patterns are robust even when search becomes too expensive</li>
</ul>

<hr>

<h2>The Ablation Story: What Makes It Work?</h2>

<p>The ablation results reveal a clear hierarchy of what matters most. Let me walk through each component:</p>

<h3>Full MCTS + Value + Policy: The Gold Standard</h3>

<p>The complete system combines three learned components with online search. Each plays a distinct role:</p>

<div class="vis-container">
    <table class="compare-table">
        <tr>
            <th>Component</th>
            <th>Role in Planning</th>
            <th>Performance Alone</th>
            <th>Contribution</th>
        </tr>
        <tr>
            <td><strong>Policy P(a|G)</strong></td>
            <td>Prioritizes actions in MCTS</td>
            <td>60-80% of full</td>
            <td>Eliminates bad actions early</td>
        </tr>
        <tr>
            <td><strong>Value V(G)</strong></td>
            <td>Evaluates leaf nodes</td>
            <td>50-70% of full</td>
            <td>Replaces expensive rollouts</td>
        </tr>
        <tr>
            <td><strong>MCTS search</strong></td>
            <td>Looks ahead from current state</td>
            <td>—</td>
            <td>Corrects network errors</td>
        </tr>
    </table>
    <p class="vis-caption">Each component provides complementary value. The policy is the strongest individual signal, but MCTS combining all three consistently produces the best results.</p>
</div>

<p>The policy network alone achieves 60-80% of full performance—demonstrating that the graph representation genuinely captures action quality. The value network performs slightly worse in isolation, which makes sense: estimating absolute values is harder than ranking relative action quality (echoing findings from fully observable planning).</p>

<p>But the combination through MCTS always wins. Search provides error correction: even when the policy's top choice is wrong, MCTS explores alternatives and uses the value network to course-correct.</p>

<hr>

<h2>Comparing Approaches: A Mental Model</h2>

<p>To understand where GammaZero sits in the landscape, here's how it compares to existing approaches:</p>

<div class="vis-container">
    <table class="compare-table">
        <tr>
            <th></th>
            <th>Fixed-Size Input</th>
            <th>Handles Uncertainty</th>
            <th>Zero-Shot Generalization</th>
            <th>Uses Search</th>
        </tr>
        <tr>
            <td><strong>POMCPOW/DESPOT</strong></td>
            <td class="yes">No (model-based)</td>
            <td class="yes">Yes</td>
            <td class="no">No (per-instance)</td>
            <td class="yes">Yes</td>
        </tr>
        <tr>
            <td><strong>BetaZero</strong></td>
            <td class="no">Yes (bottleneck)</td>
            <td class="yes">Yes</td>
            <td class="no">No</td>
            <td class="yes">Yes</td>
        </tr>
        <tr>
            <td><strong>GABAR</strong></td>
            <td class="yes">No (graphs)</td>
            <td class="no">No (fully obs.)</td>
            <td class="yes">Yes</td>
            <td class="no">No</td>
        </tr>
        <tr>
            <td><strong>GammaZero</strong></td>
            <td class="yes">No (graphs)</td>
            <td class="yes">Yes</td>
            <td class="yes">Yes</td>
            <td class="yes">Yes</td>
        </tr>
    </table>
    <p class="vis-caption">GammaZero uniquely combines variable-size graph inputs, uncertainty handling, zero-shot generalization, and online search.</p>
</div>

<p>GammaZero is essentially the POMDP analog of GABAR (our prior work on fully observable planning), but with two critical additions: belief-weighted attributes that encode uncertainty, and integration with MCTS for online error correction.</p>

<hr>

<h2>The Deeper Lessons: Invariants for Other Research</h2>

<p>Beyond the specific results, GammaZero demonstrates principles that generalize broadly:</p>

<h3>1. Structure Your Input to Match Your Problem's Structure</h3>

<p>POMDPs have relational structure: objects have attributes, actions operate on objects, beliefs assign probabilities to propositions. The graph representation preserves all of this. A flat vector discards it.</p>

<p>The principle applies beyond POMDPs: if your problem has entities, relations, and properties, your representation should make these first-class. Don't flatten structure into features—you're forcing the network to reconstruct what you already know.</p>

<h3>2. Encode Uncertainty Structurally, Not Just Numerically</h3>

<p>Instead of a feature vector [0.4, 0.6, 0.75, ...] encoding belief probabilities, GammaZero creates/removes graph nodes based on belief. The <em>topology</em> changes as beliefs change. This means:</p>

<ul>
    <li>A network can learn structural patterns ("two competing attribute nodes = high uncertainty")</li>
    <li>These patterns are scale-invariant (same local structure, regardless of graph size)</li>
    <li>The graph is naturally sparse (only plausible hypotheses get nodes)</li>
</ul>

<p>This applies to any domain with uncertainty: medical diagnosis (create symptom nodes only for plausible conditions), autonomous driving (instantiate obstacle hypotheses only above detection threshold), portfolio optimization (represent asset categories only when position is significant).</p>

<h3>3. Train Cheap, Deploy Expensive</h3>

<p>GammaZero trains on problems that cost milliseconds to solve optimally. It deploys on problems that would cost hours. The training data is essentially free—just run a planner on small instances.</p>

<p>This works because the <em>patterns are compositional</em>: how to evaluate a single rock's check-vs-sample tradeoff on a 5x5 grid is the same tradeoff on a 25x25 grid. The GNN learns these local patterns, and they compose into global policies.</p>

<p>The broader lesson: if your problem has compositional structure, you can often train on trivial instances and deploy on hard ones. The key is choosing a representation with the right inductive biases—in this case, GNNs that process local neighborhoods identically regardless of global size.</p>

<h3>4. Combine Learning with Search</h3>

<p>The policy network alone achieves 60-80% of full performance. Adding MCTS closes the gap to 100%. This isn't surprising—learned policies make mistakes, and lookahead search catches them.</p>

<p>But the reverse is also true: MCTS alone (without learned guidance) performs poorly on long-horizon POMDPs because it can't search deeply enough. The learned heuristics focus search where it matters.</p>

<p>This learning + search combination is a general recipe: learning provides fast approximate guidance, search provides deliberate error correction. Neither alone is sufficient for hard problems.</p>

<h3>5. The Global Node Trick</h3>

<p>A fixed-depth GNN (3 layers in GammaZero's case) has a limited receptive field. In a large graph, distant nodes can't communicate through local message passing alone. The global node solves this by acting as a communication hub—every node reads from it and writes to it every round.</p>

<p>This is the graph equivalent of the [CLS] token in transformers. If you're using GNNs on variable-size graphs, a global aggregation node is essentially free and dramatically improves scalability.</p>

<hr>

<h2>What This Means for Different Audiences</h2>

<h3>For POMDP/Planning Researchers</h3>

<p>GammaZero shows that graph-based generalization—previously limited to fully observable domains—can work in belief space. The belief-threshold mechanism (only instantiate nodes above tau) is a clean, principled way to handle the continuous nature of beliefs within a discrete graph framework.</p>

<p>Practical implication: you can solve "toy" versions of your POMDP domain (which are cheap), train GammaZero on those, and get reasonable policies for the full-scale version without any additional expert computation.</p>

<h3>For ML Researchers</h3>

<p>The representation design is the key contribution from an ML perspective. The insight that belief uncertainty can be encoded <em>topologically</em> (through node existence) rather than just <em>numerically</em> (through features) is powerful. It converts a continuous estimation problem into a discrete structure recognition problem—and GNNs are good at structure.</p>

<p>The graceful degradation results are also notable. Most ML models fail catastrophically on out-of-distribution inputs. GammaZero degrades <em>gracefully</em>—performance drops proportionally to the scale increase, not suddenly. This suggests the learned features are genuinely capturing transferable patterns rather than memorizing training distributions.</p>

<h3>For Robotics/Applications Researchers</h3>

<p>The practical value is clear: train once on simulation-easy instances, deploy on the real (hard) problem. For domains like robotic search, autonomous exploration, or sensor placement, this means you can develop planning heuristics without needing to solve the full-scale problem even once during development.</p>

<p>The approach also handles multiple POMDP domains with the same architecture—only the graph construction rules change. RockSample, MultiObjectSearch, and Rearrangement all use the same GNN weights with different domain-specific node/edge definitions.</p>

<hr>

<h2>Limitations and Open Questions</h2>

<p>GammaZero has clear limitations worth noting:</p>

<ul>
    <li><strong>Discrete actions and attributes:</strong> The current graph construction assumes discrete attribute values. Continuous state spaces would require discretization or a different node construction strategy.</li>
    <li><strong>Threshold sensitivity:</strong> The tau threshold for node creation is a hyperparameter. Too low creates cluttered graphs; too high misses important beliefs. Domain-adaptive thresholds could help.</li>
    <li><strong>Training data quality:</strong> Performance depends on having good expert demonstrations. For domains where even small instances are hard to solve optimally, training data generation becomes a bottleneck.</li>
    <li><strong>Performance at extreme scales:</strong> While degradation is graceful, performance does drop at 6x scale. Hierarchical graph representations or curriculum-based training might push the boundary further.</li>
</ul>

<hr>

<h2>Summary</h2>

<p>GammaZero demonstrates that representing belief states as uncertainty-aware graphs enables POMDP planners to generalize across problem scales—something no prior learning-based POMDP approach could do.</p>

<p>The recipe is concrete:</p>

<ol>
    <li>Convert particle beliefs to graphs with belief-weighted attribute nodes</li>
    <li>Train a GNN on small, cheaply-solved problem instances</li>
    <li>Use the learned policy and value function to guide MCTS on arbitrarily larger problems</li>
</ol>

<p>The result: competitive performance on same-sized problems, graceful generalization to 2-6x larger instances, and the ability to solve problems where classical planners simply timeout.</p>

<p>The broader insight: when your problem has relational structure and uncertainty, encode both in your representation—structurally, not just numerically. Let the architecture match the problem. And train cheap, deploy expensive.</p>

<hr>

<!-- Series Footer (Planning Under Uncertainty) -->
<div class="series-footer">
    <strong>Series finale</strong>
    <p>You just read the closing post of the <em>Planning Under Uncertainty</em> series. The arc: POMDPs are intractable in general (<a href="/blog/2026/planning-under-uncertainty-belief-states/">Part 1</a>); online tree search makes them solvable in practice (<a href="/blog/2026/planning-under-uncertainty-mcts-for-pomdps/">Part 2</a>); abstraction shrinks the problem until a planner can handle it (<a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">HOO-POMDP</a>); and learning grows with the problem instead of fighting it (GammaZero). The two final papers are siblings — different routes through the same partial-observability barrier.</p>
    <p style="margin-top: 10px;">If you came here from the <a href="/blog/category/learning-for-planning/">Learning for Planning</a> series, GammaZero is the natural close of the loop: same graph-based learning recipe as GABAR, now operating over beliefs instead of states.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;">← <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">Part 3: HOO-POMDP</a> · <a href="/blog/2026/planning-under-uncertainty-epilogue/">Epilogue: Open Questions →</a> · Sibling: <a href="/blog/2026/learning-for-planning-overview/">Learning for Planning</a></p>
</div>

<p><em>Paper: "GammaZero: Learning to Guide Belief-Space Search for Long-Horizon POMDPs with Generalizable Graph Representations"</em></p>
<p><em>arXiv: <a href="https://arxiv.org/abs/2510.14035">2510.14035</a></em></p>

<hr>

</article>
