---
layout: fullhtml-post
title: "Learning for Planning — Series Overview"
date: 2026-04-16
categories: ["Learning for Planning"]
tags: ["planning", "gnn", "learning"]
description: "A four-part series (plus epilogue) on how graph neural networks let learned policies generalize from small, solvable training instances to problems 8x larger — without ever retraining."
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

  .blog-fullhtml .roadmap { margin: 24px 0; padding: 22px 24px 18px; background: #fff; border: 1px solid #c2e4e3; border-radius: 12px; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .roadmap h3 { font-family: 'Playfair Display', Georgia, serif; font-size: 1.1rem; color: #00807E; margin: 0 0 14px; }
  .blog-fullhtml .rm-item { display: flex; gap: 14px; align-items: flex-start; padding: 12px 0; border-bottom: 1px solid #e8eded; }
  .blog-fullhtml .rm-item:last-child { border-bottom: none; }
  .blog-fullhtml .rm-num { font-family: 'JetBrains Mono', monospace; font-size: 1.1rem; font-weight: 700; color: #00A3A1; flex-shrink: 0; min-width: 32px; }
  .blog-fullhtml .rm-body { flex: 1; }
  .blog-fullhtml .rm-title { font-family: 'Playfair Display', Georgia, serif; font-size: 1.05rem; color: #1a1a1a; font-weight: 700; }
  .blog-fullhtml .rm-title a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }
  .blog-fullhtml .rm-title a:hover { color: #005856; }
  .blog-fullhtml .rm-desc { font-size: 0.9em; color: #555; margin-top: 3px; line-height: 1.55; }
  .blog-fullhtml .rm-tag { display: inline-block; font-size: 0.66rem; text-transform: uppercase; letter-spacing: 1.5px; font-weight: 700; color: #00807E; background: #e0f5f5; padding: 2px 8px; border-radius: 12px; margin-left: 6px; vertical-align: middle; }
  .blog-fullhtml .rm-tag.anchor { background: #00A3A1; color: #fff; }

  .blog-fullhtml .pull { background: #00807E; color: #fff; padding: 18px 22px; border-radius: 10px; margin: 28px 0; }
  .blog-fullhtml .pull p { font-size: 1.02em; line-height: 1.55; margin: 0; }
  .blog-fullhtml .pull strong { color: #f0fafa; }

  .blog-fullhtml .audience { display: flex; gap: 14px; margin: 22px 0; flex-wrap: wrap; }
  @media (max-width: 600px) { .blog-fullhtml .audience { flex-direction: column; } }
  .blog-fullhtml .aud-card { flex: 1; padding: 14px 16px; background: #f0fafa; border-radius: 8px; border: 1px solid #c2e4e3; min-width: 180px; }
  .blog-fullhtml .aud-card h4 { font-family: 'Playfair Display', Georgia, serif; font-size: 0.98rem; color: #00807E; margin: 0 0 6px; }
  .blog-fullhtml .aud-card p { font-size: 0.86em; color: #444; line-height: 1.5; margin: 0; }

  .blog-fullhtml .blog-container { max-width: 680px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }
  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3a3a; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #0f2625; border-left-color: #1e4a48; color: #b0c8c6; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #142a2a; border-color: #1e4a48; color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.3); }
  html[data-theme="dark"] .blog-fullhtml .roadmap { background: #1a2530; border-color: #2a4545; }
  html[data-theme="dark"] .blog-fullhtml .roadmap h3 { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .rm-item { border-bottom-color: #2a3a3a; }
  html[data-theme="dark"] .blog-fullhtml .rm-num { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .rm-title { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .rm-title a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.3); }
  html[data-theme="dark"] .blog-fullhtml .rm-title a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .rm-desc { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml .rm-tag { background: #1e3a3a; color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .rm-tag.anchor { background: #4dd0ce; color: #0a1818; }
  html[data-theme="dark"] .blog-fullhtml .pull { background: #1e4a48; color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .pull strong { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .aud-card { background: #142a2a; border-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .aud-card h4 { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .aud-card p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Learning for Planning &middot; Series Overview</strong>
    <div class="series-nav-links">
        You are here. The series begins at <a href="/blog/2026/learning-for-planning-scaling-problem/">Part 1 &rarr;</a>
    </div>
</div>

<h1>Learning for Planning</h1>
<p class="subtitle">A four-part series (plus epilogue) on how graph neural networks let learned policies generalize from small, solvable training instances to problems 8&times; larger &mdash; without ever retraining.</p>

<hr>

<h2>Why this series exists</h2>

<p>Classical planning &mdash; the framework underneath everything from Fast-Downward to LAMA &mdash; is sound, complete, and optimal. It is also exponentially hard. Every domain-independent planner hits a wall as problem size grows. The wall is not a bug; it is a theorem.</p>

<p>For two decades, the natural fix has been to train a value function on solved instances and use it to guide search on harder ones. The fix mostly hasn't worked. Values learned for small problems don't transfer to large ones, because the input representation itself depends on the problem size.</p>

<p>This series walks through why that's the wrong recipe and what to do instead: <em>frame the planning state as a graph, rank actions instead of states, and let a graph neural network ingest variable-size problems with the same weights.</em> The series anchors on <strong>GABAR</strong>, our NeurIPS 2025 paper, which puts the recipe together and shows generalization to instances 8&times; the size of anything seen during training.</p>

<div class="pull">
    <p><strong>One unifying example.</strong> All four posts use the same toy domain &mdash; a robot delivering packages between zones in a warehouse. Each post returns to it, scaled differently. Post 1 animates the jump from 4 zones and 1 package (solvable in milliseconds by a classical planner) to 16 zones and 3 packages (the start of the exponential wall). By Post 4 the same recipe is solving instances 8&times; larger than anything it trained on &mdash; 100+ objects &mdash; while the learned baselines and LLMs collapse.</p>
</div>

<h2>The roadmap</h2>

<div class="roadmap">
    <h3>Four parts + an epilogue, one paper, one running example</h3>

    <div class="rm-item">
        <div class="rm-num">01</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/learning-for-planning-scaling-problem/">The Scaling Problem</a> <span class="rm-tag">the problem + framing</span></div>
            <div class="rm-desc">Why classical planners hit an exponential wall, with an animated 4-zone vs 16-zone warehouse. Then introduces the two design axes &mdash; <em>what to learn</em> (heuristics, value functions, ranking) and <em>how to represent state</em> (graph encoding choices) &mdash; that organize every paper in the rest of the series.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">02</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/learning-for-planning-what-to-learn/">What to Learn &mdash; A Survey of Learning Objectives</a> <span class="rm-tag">survey: axis 1</span></div>
            <div class="rm-desc">Full survey of Axis 1. Three families: heuristic learning (ASNets, STRIPS-HGN, GOOSE), value-function learning (GPL, expressive variants), and ranking (RankSVM, Chrestien et al., GBFS-rank, GRAPL). Each paper gets concrete coverage: what they did, what they showed, what they couldn't do.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">03</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/learning-for-planning-state-as-graph/">Your Planning State Is a Graph</a> <span class="rm-tag">survey: axis 2</span></div>
            <div class="rm-desc">Full survey of Axis 2, opening with an interactive 5-stage graph-construction walkthrough. Three sub-decisions: lifted vs grounded encoding (the GOOSE debate), action representation (implicit vs alternating layers vs explicit nodes vs hypergraph), and parameter construction (independent vs sequential decoding). Comparison table across the literature.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">04</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/learning-for-planning-gabar/">GABAR &mdash; You Don't Need to Rank All States. Just Rank the Actions in Front of You.</a> <span class="rm-tag anchor">paper deep-dive</span></div>
            <div class="rm-desc">The full GABAR architecture. Action-centric graph + GNN encoder + GRU decoder for conditional decoding of grounded actions. Interactive demo of the greedy execution loop on the running warehouse. Ablations, results across 8 IPC domains, and what the experiments actually tell us about which design choices matter.</div>
        </div>
    </div>

    <div class="rm-item">
        <div class="rm-num">EP</div>
        <div class="rm-body">
            <div class="rm-title"><a href="/blog/2026/learning-for-planning-epilogue/">Epilogue &mdash; Toward Uncertainty</a> <span class="rm-tag">looking forward</span></div>
            <div class="rm-desc">What GABAR can't do, what's coming, and the natural next chapter: <em>partial observability</em>. The sibling series picks up the same warehouse but with the robot's view fogged out. A short bridge to <a href="/blog/category/planning-under-uncertainty/">Planning Under Uncertainty</a>.</div>
        </div>
    </div>
</div>

<h2>Who this is for</h2>

<div class="audience">
    <div class="aud-card">
        <h4>The classical planning reader</h4>
        <p>You know PDDL, A*, and heuristic search. You've watched the field flirt with learning for decades. This series tells you why it finally clicks &mdash; and where you should still be skeptical.</p>
    </div>
    <div class="aud-card">
        <h4>The ML reader</h4>
        <p>You know GNNs, transformers, attention. You don't know why action ranking and graph representations matter <em>specifically</em> for planning. Post 2 and Post 3 are for you. Post 4 is the experiment.</p>
    </div>
    <div class="aud-card">
        <h4>The robotics reader</h4>
        <p>You want long-horizon plans that scale to real-world manipulation problems. Read all four. The epilogue is the bridge to partial observability, which is where most of your real problems actually live.</p>
    </div>
</div>

<h2>Reading paths</h2>

<ul>
    <li><strong>Linear (recommended for first read):</strong> 01 &rarr; 02 &rarr; 03 &rarr; 04 &rarr; Epilogue. About 90 minutes total, including time spent on the interactive visualizations.</li>
    <li><strong>Paper-first:</strong> Skip straight to 04 (GABAR) for the paper deep-dive. The "Where GABAR sits in the literature" note near the top of that post points back to Posts 1-3 if you need the setup.</li>
    <li><strong>Concept tour:</strong> Just Posts 1 and 4. The scaling problem (Post 1) and the paper that solves it (Post 4). Posts 2 and 3 are supporting material.</li>
</ul>

<h2>What you won't find here</h2>

<ul>
    <li><strong>No PDDL tutorial.</strong> If you need one, posts 2-3 of <a href="/blog/category/llms-automated-planning-and-agents/">Planning in the Era of LLMs</a> cover that.</li>
    <li><strong>No deep-RL primer.</strong> The series treats Q-learning, policy gradients, and AlphaZero as known reference points but doesn't re-teach them.</li>
    <li><strong>No reinforcement-learning angle.</strong> GABAR learns from a planner's demonstrations, not from environment rewards. The interplay between RL and classical planning is left for another day.</li>
</ul>

<hr>

<div class="series-footer">
    <strong>Ready?</strong>
    <p>Start at <a href="/blog/2026/learning-for-planning-scaling-problem/">Part 1: The Scaling Problem</a>. Or jump straight to <a href="/blog/2026/learning-for-planning-gabar/">Part 4: GABAR</a> for the paper. Or, if you want the partially-observable cousin first, drift over to <a href="/blog/2026/planning-under-uncertainty-overview/">Planning Under Uncertainty</a>.</p>
</div>

</article>
