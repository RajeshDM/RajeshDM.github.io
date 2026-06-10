---
layout: fullhtml-post
title: "MCTS for POMDPs"
date: 2026-04-23
categories: ["Planning under Uncertainty"]
tags: ["planning", "pomdp", "mcts"]
description: "Exact POMDPs are intractable. Online tree search makes them solvable in practice. The price: every modern POMDP solver depends on heuristics — and that's exactly where learning fits in. Part 2 of the Planning Under Uncertainty series."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml p { margin: 0 0 1.3em; }
  .blog-fullhtml ul, .blog-fullhtml ol { margin: 0 0 1.3em; }
  .blog-fullhtml h1 { font-size: 2em; line-height: 1.2; margin-top: 1.2em; }
  .blog-fullhtml h2 { font-size: 1.5em; margin-top: 2em; color: #222; }
  .blog-fullhtml h3 { font-size: 1.2em; margin-top: 1.5em; }
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

  .blog-fullhtml .vis-container {
  margin: 28px -40px; padding: 22px 24px 14px;
  background: #fff; border: 1px solid #E8E4ED;
  border-radius: 14px; box-shadow: 0 4px 18px rgba(45,32,68,0.06);
  font-family: 'Source Sans 3', -apple-system, 'Helvetica Neue', Arial, sans-serif;
  }
  @media (max-width: 800px) { .blog-fullhtml .vis-container { margin: 24px 0; } }
  .blog-fullhtml .vis-title { font-family: 'Playfair Display', Georgia, serif; font-size: 1.2rem; color: #2D2044; font-weight: 700; margin: 0; }
  .blog-fullhtml .vis-title::after { content:''; display:block; width:60px; height:3px; background:#C5A55A; margin-top:7px; border-radius:2px; }
  .blog-fullhtml .vis-subtitle { color: #888; font-size: 0.9em; margin-top: 6px; margin-bottom: 14px; font-style: italic; }

  .blog-fullhtml .phase-row { display: flex; gap: 8px; margin-bottom: 14px; flex-wrap: wrap; }
  .blog-fullhtml .phase-pill { font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; font-weight: 700; padding: 5px 10px; border-radius: 16px; background: #F8F6FA; color: #6B5B7B; border: 1px solid #E8E4ED; cursor: pointer; transition: all 0.2s; }
  .blog-fullhtml .phase-pill.active { background: #5b6abf; color: #fff; border-color: #5b6abf; }
  .blog-fullhtml .phase-pill:hover:not(.active) { background: #ECE7F2; }
  .blog-fullhtml .phase-desc { font-size: 0.92em; color: #2D2044; line-height: 1.5; padding: 10px 14px; background: #F8F6FA; border-left: 3px solid #5b6abf; border-radius: 0 6px 6px 0; margin-bottom: 14px; min-height: 44px; }
  .blog-fullhtml .phase-desc strong { color: #3d4a9e; }

  .blog-fullhtml .tree-frame { background: #FDFCFE; border: 1px solid #D4CDE0; border-radius: 8px; padding: 12px; overflow: hidden; }
  .blog-fullhtml .tree-frame svg { width: 100%; height: auto; }

  .blog-fullhtml .blog-container { max-width: 680px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3045; }
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
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1e1a30; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-title { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .vis-subtitle { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .phase-pill { background: #1a1530; color: #a8b8b8; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .phase-pill:hover:not(.active) { background: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .phase-desc { color: #c5aae8; background: #1a1530; }
  html[data-theme="dark"] .blog-fullhtml .phase-desc strong { color: #9aafe6; }
  html[data-theme="dark"] .blog-fullhtml .tree-frame { background: #15101e; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Planning Under Uncertainty &middot; Part 2 of 4 &mdash; the landscape</strong>
    <div class="series-nav-links">
        &larr; <a href="/blog/2026/planning-under-uncertainty-belief-states/">Part 1: Planning When You Can't See the Whole World</a> &middot; Next: <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">Part 3: HOO-POMDP &rarr;</a>
    </div>
</div>

<h1>MCTS for POMDPs</h1>
<p class="subtitle">Exact POMDPs are intractable. Online tree search makes them solvable in practice. The price: every modern POMDP solver depends on heuristics &mdash; and that's exactly where learning fits in.</p>

<hr>

<p>Part 1 left us with a problem: belief space is infinite-dimensional, exact dynamic programming over it is hopeless, and even ε-approximate solutions are intractable for most non-trivial POMDPs. This post covers the workaround the field converged on, the algorithmic vocabulary the remaining two posts in this series depend on, and the gap that learning is meant to fill.</p>

<h2>Stop solving the whole problem</h2>

<p>The shift is from <em>offline</em> to <em>online</em> planning. Don't try to compute a global policy over all of belief space &mdash; you can't. Instead, at every step, build a small search tree from the current belief, pick the action that looks best in that tree, execute it, observe what happens, and repeat. The tree is small and disposable; the next step you build a fresh one from the new belief.</p>

<p>The tree's root is the agent's current belief. Each branch represents an action followed by some observation the world might produce. The whole tree is therefore an action-observation chain &mdash; a <em>history</em>.</p>

<h2>Particle beliefs</h2>

<p>Before we get to the algorithm, one representational trick. We never represent beliefs as explicit distributions over states &mdash; that would be intractable for any non-toy state space. Instead we keep a <em>particle filter</em>: a collection of sampled states drawn from the current belief. Updating the belief after an observation becomes weighting and resampling particles. This makes everything tractable in practice, because the bottleneck shifts from "represent the belief" to "sample enough particles."</p>

<p>You'll see this assumption baked into every solver below. The same warehouse Part 1 used: the agent doesn't know where the three packages are, but it has a few hundred candidate world configurations &mdash; particles &mdash; consistent with what it has observed so far.</p>

<h2>POMCP &mdash; the canonical online POMDP solver</h2>

<p><strong>Partially Observable Monte Carlo Planning</strong> (Silver and Veness, 2010) is the workhorse. It adapts UCT &mdash; the same algorithm behind early AlphaGo &mdash; to belief space. The four phases of MCTS, slightly retargeted, look like this:</p>

<div class="vis-container">
    <h3 class="vis-title">POMCP &mdash; the four-phase loop</h3>
    <div class="vis-subtitle">Same warehouse as Part 1, now foggy. Root is the current belief; tree branches over (action, observation) pairs.</div>

    <div class="phase-row" id="phase-row">
        <div class="phase-pill active" data-p="0">1 &middot; Selection</div>
        <div class="phase-pill" data-p="1">2 &middot; Expansion</div>
        <div class="phase-pill" data-p="2">3 &middot; Rollout</div>
        <div class="phase-pill" data-p="3">4 &middot; Backprop</div>
    </div>

    <div class="phase-desc" id="phase-desc"><strong>Selection.</strong> From the root belief, walk down the existing tree by picking actions according to UCB1 (favor high-value actions, but also under-explored ones). At each node you also sample a state from the particle belief and step it forward to choose which observation branch to follow.</div>

    <div class="tree-frame">
        <svg id="vis-tree" viewBox="0 0 560 270" preserveAspectRatio="xMidYMid meet"></svg>
    </div>
</div>

<script>
(function(){
    var nodes = [
        { id:'root', x:280, y:30, l:'b' },
        { id:'a1', x:130, y:100, l:'a₁' },
        { id:'a2', x:280, y:100, l:'a₂' },
        { id:'a3', x:430, y:100, l:'a₃' },
        { id:'o11', x:80, y:170, l:'o' },
        { id:'o12', x:180, y:170, l:'o' },
        { id:'o21', x:230, y:170, l:'o' },
        { id:'o22', x:330, y:170, l:'o' },
        { id:'o31', x:380, y:170, l:'o' },
        { id:'o32', x:480, y:170, l:'o' },
        { id:'leaf', x:180, y:240, l:'b\'' }
    ];

    var phases = [
        {
            desc: '<strong>Selection.</strong> From the root belief, walk down the existing tree by picking actions according to UCB1 (favor high-value actions, but also under-explored ones). At each node you also sample a state from the particle belief and step it forward to choose which observation branch to follow.',
            edges: [['root','a1','sel'],['a1','o12','sel']],
            nodes: { root:'sel', a1:'sel', o12:'sel' }
        },
        {
            desc: '<strong>Expansion.</strong> When you reach a leaf &mdash; an action-observation history not yet in the tree &mdash; create a new node for it. Initialize its particle belief by filtering the parent\'s particles through the action and observation.',
            edges: [['root','a1','sel'],['a1','o12','sel'],['o12','leaf','new']],
            nodes: { root:'sel', a1:'sel', o12:'sel', leaf:'new' }
        },
        {
            desc: '<strong>Rollout.</strong> From the new leaf, simulate forward with a cheap default policy (often random) until you hit a terminal condition or a horizon. The total return is a one-sample estimate of the leaf\'s value &mdash; this is where heuristic ingredients matter most.',
            edges: [['root','a1','sel'],['a1','o12','sel'],['o12','leaf','new']],
            nodes: { root:'sel', a1:'sel', o12:'sel', leaf:'roll' }
        },
        {
            desc: '<strong>Backpropagation.</strong> Push the rollout return back up through every node visited. Visit counts and value estimates update, and the next iteration\'s selection step uses the new statistics. Repeat thousands of times within the per-step time budget.',
            edges: [['root','a1','bp'],['a1','o12','bp'],['o12','leaf','bp']],
            nodes: { root:'bp', a1:'bp', o12:'bp', leaf:'bp' }
        }
    ];

    var col = { def: '#D4CDE0', dim: '#E8E4ED', sel: '#5b6abf', selBg:'#ecedfa', new: '#1E8449', newBg:'#E3F5EC', roll: '#E67E22', rollBg:'#FFF5E6', bp: '#C0392B', bpBg:'#FDECEA', text:'#2D2044' };

    function render(p){
        var phase = phases[p];
        document.getElementById('phase-desc').innerHTML = phase.desc;
        document.querySelectorAll('#phase-row .phase-pill').forEach(function(el){
            el.classList.toggle('active', parseInt(el.getAttribute('data-p')) === p);
        });

        var NM = {}; nodes.forEach(function(n){ NM[n.id] = n; });

        var s = '';
        // base edges (dimmed)
        var base = [['root','a1'],['root','a2'],['root','a3'],['a1','o11'],['a1','o12'],['a2','o21'],['a2','o22'],['a3','o31'],['a3','o32']];
        base.forEach(function(e){
            var f = NM[e[0]], t = NM[e[1]];
            var override = phase.edges.find(function(pe){ return pe[0]===e[0] && pe[1]===e[1]; });
            var color = override ? col[override[2]] : col.dim;
            var width = override ? 2.5 : 1;
            s += '<line x1="' + f.x + '" y1="' + f.y + '" x2="' + t.x + '" y2="' + t.y + '" stroke="' + color + '" stroke-width="' + width + '" opacity="' + (override ? 0.85 : 0.5) + '"/>';
        });
        // phase-specific edges that aren't in base (e.g., to new leaf)
        phase.edges.forEach(function(pe){
            var found = base.find(function(be){ return be[0]===pe[0] && be[1]===pe[1]; });
            if (found) return;
            var f = NM[pe[0]], t = NM[pe[1]];
            s += '<line x1="' + f.x + '" y1="' + f.y + '" x2="' + t.x + '" y2="' + t.y + '" stroke="' + col[pe[2]] + '" stroke-width="2.5" stroke-dasharray="' + (pe[2]==='new'?'5 3':'none') + '"/>';
        });

        // nodes
        nodes.forEach(function(n){
            var state = phase.nodes[n.id];
            var c, bg;
            if (state) { c = col[state]; bg = col[state + 'Bg']; }
            else { c = col.def; bg = '#fff'; }
            var isRoot = (n.id === 'root');
            var r = isRoot ? 16 : 13;
            s += '<circle cx="' + n.x + '" cy="' + n.y + '" r="' + r + '" fill="' + bg + '" stroke="' + c + '" stroke-width="' + (state ? 2 : 1.2) + '"' + (state==='new'?' stroke-dasharray="4 2"':'') + '/>';
            s += '<text x="' + n.x + '" y="' + (n.y + 4) + '" text-anchor="middle" fill="' + (state ? c : col.text) + '" font-size="' + (isRoot ? 12 : 11) + '" font-weight="700" font-family="JetBrains Mono, monospace">' + n.l + '</text>';
        });

        // legend
        s += '<text x="14" y="265" fill="#888" font-size="10" font-style="italic">root = belief &middot; level 1 = actions &middot; level 2 = observations &middot; new leaves get new beliefs</text>';

        document.getElementById('vis-tree').innerHTML = s;
    }

    document.querySelectorAll('#phase-row .phase-pill').forEach(function(el){
        el.addEventListener('click', function(){ render(parseInt(el.getAttribute('data-p'))); });
    });
    render(0);
})();
</script>

<h2>DESPOT &mdash; the "determinized" alternative</h2>

<p><strong>Determinized Sparse Partially Observable Tree</strong> (Ye, Somani, Hsu, and Lee, 2017) takes a different tack. Instead of sampling histories on the fly during search, DESPOT samples a small set of <em>scenarios</em> &mdash; entire deterministic worlds drawn from the initial belief &mdash; up front. The search tree is then built over those scenarios, with explicit regularization to avoid overfitting to the particular sample. The regularization coefficient trades tree size against estimated value, and theoretical analysis shows the resulting plan is near-optimal with high probability when the scenario sample is sufficient.</p>

<p>POMCP samples observations on the fly; DESPOT commits to a sample up front and searches more efficiently within it. Both have become standard baselines.</p>

<h2>POMCPOW &mdash; continuous observations via progressive widening</h2>

<p><strong>POMCPOW</strong> (Sunberg and Kochenderfer, 2018) addresses one of POMCP's worst pathologies: continuous observation spaces. Vanilla POMCP creates a separate tree branch for every distinct observation encountered during simulation, so when observations are drawn from a continuous space &mdash; sensor readings, real-valued positions &mdash; every sample produces a new branch and the tree never gets to revisit anything. Progressive widening caps the number of observation branches at each node as a function of the visit count, adding new branches only when the existing ones have been explored enough. The tree stays compact without throwing away the statistical benefit of continuous sampling.</p>

<p>POMCPOW is the right choice when observations are high-dimensional or continuous &mdash; which is much of robotics, where a camera frame is the observation.</p>

<h2>AdaOPS &mdash; adaptive bounds</h2>

<p><strong>AdaOPS</strong> (Wu et al., 2021) combines particle filtering with the adaptive branching strategy of heuristic search planners. At each tree node it maintains <em>upper and lower bounds</em> on the value function and refines those bounds through selective expansion. The bounds let AdaOPS prune clearly suboptimal actions early, focusing simulation effort on actions whose bounds still overlap. It is the bound-driven cousin of DESPOT and tends to be the strongest non-learning baseline on benchmark POMDP domains.</p>

<p>POMCP, POMCPOW, DESPOT, AdaOPS &mdash; together these define the current ceiling of non-learning online POMDP planning. They differ in sampling strategy and pruning mechanism but agree on the basic loop: build a tree from the current belief, decide via UCB-like statistics, execute the top action, re-plan from the new belief. They all share the same dependency on rollouts to evaluate leaf nodes.</p>

<h2>The dependency that learning can replace</h2>

<p>All four methods above need a way to assign a value to a leaf node. None of them computes that value exactly &mdash; the whole point of online planning is to avoid the exponential cost of exact computation. So they substitute. Random rollouts simulate forward from the leaf with a default policy until reaching a terminal or a horizon, and use the discounted return as the value estimate. Or they use hand-crafted bounds (AdaOPS) or hand-crafted heuristics (everything else when random rollouts aren't enough).</p>

<p>This is the crucial bottleneck. Random rollouts are unbiased but extremely high-variance &mdash; a single sample of the future barely tells you anything about a hard problem, so the planner needs many simulations per decision before the estimates stabilize. The HOO-POMDP experiments in Chapter~3 of the thesis make this concrete: at 20 objects, the planner spends nearly half an hour per task, and most of that cost is rollout variance. The state abstraction in HOO-POMDP reduces the effective state space but does not remove the rollout dependency. POMCP still runs at every decision, still needs many particles, still rolls out from leaves.</p>

<p>The alternative is to learn what the rollouts are trying to estimate. Train a value network that predicts the leaf value directly from the belief, in a single forward pass. Train an action prior that biases the search toward promising actions. Both replace the noisy, expensive rollout estimate with a deterministic, cheap network call. This is exactly the AlphaZero recipe for board games &mdash; and exactly what GammaZero (Part 4) ports to POMDPs.</p>

<div class="vis-container">
    <h3 style="font-family:'Playfair Display',Georgia,serif; font-size:1.2rem; color:#2D2044; font-weight:700; margin:0;">Back to the foggy warehouse &mdash; what the rollout bottleneck looks like</h3>
    <p style="color:#888; font-size:0.92em; margin-top:6px; margin-bottom:14px; font-style:italic;">Same foggy warehouse from Part 1. At every decision step, POMCP grows a tree from the current belief, then random-rolls every leaf to estimate its value. The leaves are where time goes.</p>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#5b6abf; margin-bottom:6px;">Current belief over the foggy warehouse</div>
            <svg viewBox="0 0 280 200" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs>
                    <radialGradient id="fogPUU2" cx="50%" cy="50%" r="55%"><stop offset="0%" stop-color="#2E1A38" stop-opacity=".25"/><stop offset="100%" stop-color="#2E1A38" stop-opacity=".55"/></radialGradient>
                </defs>
                <rect x="10" y="6" width="260" height="100" rx="4" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".5"/>
                <rect x="12" y="8" width="128" height="46" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <text x="76" y="34" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">Zone A</text>
                <rect x="142" y="8" width="128" height="46" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <text x="206" y="34" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">Zone B</text>
                <rect x="12" y="58" width="128" height="46" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <text x="76" y="84" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">Zone C</text>
                <rect x="142" y="58" width="128" height="46" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".4"/>
                <text x="206" y="84" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">Zone D</text>
                <rect x="142" y="8" width="128" height="46" fill="url(#fogPUU2)"/>
                <rect x="12" y="58" width="128" height="46" fill="url(#fogPUU2)"/>
                <rect x="142" y="58" width="128" height="46" fill="url(#fogPUU2)"/>

                <!-- Robot visible in A -->
                <g transform="translate(76,28)"><rect x="-5" y="-7" width="10" height="6" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-6" y="-2" width="12" height="9" rx="1.4" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>

                <!-- Particles (small dots) representing belief -->
                <circle cx="200" cy="35" r="2" fill="#D68910" opacity=".7"/>
                <circle cx="210" cy="38" r="2" fill="#D68910" opacity=".7"/>
                <circle cx="60" cy="80" r="2" fill="#D68910" opacity=".7"/>
                <circle cx="75" cy="85" r="2" fill="#D68910" opacity=".7"/>
                <circle cx="80" cy="78" r="2" fill="#D68910" opacity=".7"/>
                <circle cx="180" cy="80" r="2" fill="#D68910" opacity=".7"/>
                <circle cx="220" cy="85" r="2" fill="#D68910" opacity=".7"/>

                <text x="140" y="124" text-anchor="middle" fill="#5b6abf" font-size="10" font-weight="700">Particle belief: ~200 candidate worlds</text>
                <text x="140" y="138" text-anchor="middle" fill="#888" font-size="9" font-style="italic">"package is somewhere in B/C/D"</text>

                <rect x="20" y="150" width="240" height="42" rx="4" fill="#ecedfa" stroke="#5b6abf" stroke-width="1.4"/>
                <text x="140" y="167" text-anchor="middle" fill="#3d4a9e" font-size="10" font-weight="700">Each tree leaf is rolled out randomly</text>
                <text x="140" y="181" text-anchor="middle" fill="#3d4a9e" font-size="9" font-style="italic">until a goal is met or horizon is reached</text>
            </svg>
        </div>
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#C0392B; margin-bottom:6px;">Where the time goes</div>
            <svg viewBox="0 0 280 200" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <!-- Tree skeleton -->
                <line x1="140" y1="20" x2="50" y2="55" stroke="#D4CDE0" stroke-width="1.2"/>
                <line x1="140" y1="20" x2="140" y2="55" stroke="#D4CDE0" stroke-width="1.2"/>
                <line x1="140" y1="20" x2="230" y2="55" stroke="#D4CDE0" stroke-width="1.2"/>
                <line x1="50" y1="65" x2="30" y2="100" stroke="#D4CDE0" stroke-width="1"/>
                <line x1="50" y1="65" x2="70" y2="100" stroke="#D4CDE0" stroke-width="1"/>
                <line x1="140" y1="65" x2="120" y2="100" stroke="#D4CDE0" stroke-width="1"/>
                <line x1="140" y1="65" x2="160" y2="100" stroke="#D4CDE0" stroke-width="1"/>
                <line x1="230" y1="65" x2="210" y2="100" stroke="#D4CDE0" stroke-width="1"/>
                <line x1="230" y1="65" x2="250" y2="100" stroke="#D4CDE0" stroke-width="1"/>

                <!-- Root -->
                <circle cx="140" cy="16" r="10" fill="#5b6abf"/>
                <text x="140" y="20" text-anchor="middle" fill="#fff" font-size="9" font-weight="700">b</text>

                <!-- Level 1 (action nodes) -->
                <circle cx="50" cy="60" r="7" fill="#fff" stroke="#5b6abf" stroke-width="1"/>
                <circle cx="140" cy="60" r="7" fill="#fff" stroke="#5b6abf" stroke-width="1"/>
                <circle cx="230" cy="60" r="7" fill="#fff" stroke="#5b6abf" stroke-width="1"/>

                <!-- Level 2 (leaves) -->
                <circle cx="30" cy="105" r="9" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <circle cx="70" cy="105" r="9" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <circle cx="120" cy="105" r="9" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <circle cx="160" cy="105" r="9" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <circle cx="210" cy="105" r="9" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <circle cx="250" cy="105" r="9" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>

                <!-- Rollout indicators (squiggly lines extending from leaves) -->
                <path d="M30,114 Q24,130 30,140 Q36,150 30,160" stroke="#C0392B" stroke-width=".8" fill="none" stroke-dasharray="2 2"/>
                <path d="M70,114 Q76,130 70,140 Q64,150 70,160" stroke="#C0392B" stroke-width=".8" fill="none" stroke-dasharray="2 2"/>
                <path d="M120,114 Q114,130 120,140 Q126,150 120,160" stroke="#C0392B" stroke-width=".8" fill="none" stroke-dasharray="2 2"/>
                <path d="M160,114 Q166,130 160,140 Q154,150 160,160" stroke="#C0392B" stroke-width=".8" fill="none" stroke-dasharray="2 2"/>
                <path d="M210,114 Q204,130 210,140 Q216,150 210,160" stroke="#C0392B" stroke-width=".8" fill="none" stroke-dasharray="2 2"/>
                <path d="M250,114 Q256,130 250,140 Q244,150 250,160" stroke="#C0392B" stroke-width=".8" fill="none" stroke-dasharray="2 2"/>

                <text x="140" y="180" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">~6 leaves × ~50 rollouts each ≈ 300 simulations</text>
                <text x="140" y="194" text-anchor="middle" fill="#C0392B" font-size="9" font-style="italic">per decision · all for one action choice</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #ecedfa; border-radius: 8px; border-left: 4px solid #5b6abf;">
        <p style="font-size: 0.92em; color: #3a4475; line-height: 1.5; margin: 0;"><strong>Why this matters for what comes next.</strong> Every red squiggly above is a random forward simulation. Hundreds of them, per decision, for every step of the plan. The HOO-POMDP planner (Part 3) keeps this loop but shrinks the state space; the rollouts still happen. GammaZero (Part 4) removes them &mdash; the value network predicts what each rollout would have returned, in a single forward pass.</p>
    </div>
</div>

<h2>From "two strategies" to "abstraction reaches its limit, learning takes over"</h2>

<p>Earlier framings of this series suggested two parallel research strategies for scaling POMDPs: <em>abstraction</em> and <em>learning</em>. With the heuristic-gap story above, the framing tightens.</p>

<ul>
    <li><strong>Abstraction (HOO-POMDP, Part 3)</strong> reduces the effective state space the planner has to reason over. It scales the problem to tractable sizes by collapsing irrelevant detail. But once the abstraction is in place, the inner loop is still POMCP &mdash; with random rollouts. Abstraction alone hits a wall: the rollouts become the dominant cost.</li>
    <li><strong>Learning (GammaZero, Part 4)</strong> attacks the bottleneck directly. The rollouts are replaced by a learned value network; the search is biased by a learned policy. Crucially, the network is graph-based, so it generalizes across instance sizes &mdash; the same network can guide search on a 4-zone warehouse and a 50-zone warehouse without retraining.</li>
</ul>

<p>HOO-POMDP (Part 3) shows how far principled abstraction gets us. It scales to 20 objects, solves complex multi-room rearrangement under partial observability &mdash; but slowly. GammaZero (Part 4) takes the next step: same kind of problem, but with the rollout bottleneck removed, scaling improves significantly and per-decision cost drops by orders of magnitude. The story is sequential, not parallel.</p>

<hr>

<p>With this post's MCTS skeleton in hand, the rest of the series follows the arc. Part 3 (HOO-POMDP) shows how far principled abstraction gets us &mdash; impressive, but bottlenecked by rollouts. Part 4 (GammaZero) removes the bottleneck via learning. Same warehouse with fog throughout.</p>

<hr>

<div class="series-footer">
    <strong>Where this fits</strong>
    <p>This is the toolkit. POMCP, DESPOT, POMCPOW, and AdaOPS are the non-learning baselines GammaZero (Part 4) competes against. HOO-POMDP (Part 3) wraps these solvers with an abstraction layer to scale; GammaZero replaces the rollouts inside the solver with learned guidance.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;">&larr; <a href="/blog/2026/planning-under-uncertainty-belief-states/">Part 1: POMDP Setup</a> &middot; <a href="/blog/2026/planning-under-uncertainty-hoo-pomdp/">Part 3: HOO-POMDP (abstraction) &rarr;</a></p>
</div>

</article>
