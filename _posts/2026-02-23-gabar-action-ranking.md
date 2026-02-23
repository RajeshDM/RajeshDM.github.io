---
layout: fullhtml-post
title: "You Don't Need to Rank All States. Just Rank the Actions in Front of You."
date: 2026-02-23
categories: ["My Research"]
tags: ["planning", "gnn", "action-ranking", "generalization"]
description: "How a simple shift in learning objective—from global value functions to local action ranking—yields planning policies that generalize 8x beyond training size. A deep dive into GABAR."
_styles: >
  .blog-fullhtml *, .blog-fullhtml *::before, .blog-fullhtml *::after { box-sizing: border-box; margin: 0; padding: 0; }
  .blog-fullhtml { font-family: 'Georgia', 'Times New Roman', serif; line-height: 1.8; color: #1a1a2e; }
  .blog-fullhtml .hero { background: linear-gradient(135deg, #004d40 0%, #00695c 40%, #00897b 100%); color: #f0f0f0; padding: 80px 20px 60px; text-align: center; }
  .blog-fullhtml .hero .series-label { font-family: -apple-system, sans-serif; font-size: 0.85rem; text-transform: uppercase; letter-spacing: 3px; color: #80cbc4; margin-bottom: 16px; }
  .blog-fullhtml .hero h1 { font-size: 2.2rem; font-weight: 700; line-height: 1.25; max-width: 820px; margin: 0 auto 20px; color: #f0f0f0; border: none; padding: 0; }
  .blog-fullhtml .hero .subtitle { font-size: 1.05rem; color: #b2dfdb; max-width: 680px; margin: 0 auto 28px; font-style: italic; }
  .blog-fullhtml .blog-container { max-width: 780px; margin: 0 auto; padding: 48px 24px 80px; }
  .blog-fullhtml h2 { font-size: 1.85rem; color: #004d40; margin: 56px 0 20px; padding-bottom: 8px; border-bottom: 3px solid #00897b; }
  .blog-fullhtml h3 { font-size: 1.35rem; color: #00695c; margin: 40px 0 14px; }
  .blog-fullhtml p { margin-bottom: 18px; font-size: 1.05rem; }
  .blog-fullhtml strong { color: #1a1a2e; }
  .blog-fullhtml a { color: #00897b; text-decoration: none; border-bottom: 1px solid rgba(0,137,123,0.3); }
  .blog-fullhtml a:hover { color: #004d40; border-bottom-color: #004d40; }
  .blog-fullhtml ul, .blog-fullhtml ol { margin: 0 0 20px 28px; font-size: 1.05rem; }
  .blog-fullhtml li { margin-bottom: 6px; }
  .blog-fullhtml blockquote { border-left: 3px solid #00897b; margin: 1.5em 0; padding: 0.5em 1.5em; color: #444; font-style: italic; background: #e0f2f1; border-radius: 0 6px 6px 0; }
  .blog-fullhtml hr { border: none; border-top: 1px solid #ddd; margin: 2.5em 0; }
  .blog-fullhtml code { background: #f4f4f4; padding: 2px 5px; border-radius: 3px; font-size: 0.9em; }
  .blog-fullhtml .equation-note { background: #f0f4ff; border: 1px dashed #aac; padding: 10px 15px; margin: 1em 0; border-radius: 4px; font-family: monospace; font-size: 0.85em; color: #336; }
  .blog-fullhtml .equation-note::before { content: "EQUATION (use Substack's equation button): "; font-weight: bold; color: #558; }
  .blog-fullhtml .results-chart-container { margin: 25px 0; padding: 20px; background: #fafafa; border-radius: 10px; }
  .blog-fullhtml .chart { display: flex; flex-direction: column; gap: 6px; }
  .blog-fullhtml .chart-row { display: flex; align-items: center; gap: 10px; }
  .blog-fullhtml .chart-label { width: 130px; font-size: 0.8em; font-weight: 600; color: #444; text-align: right; flex-shrink: 0; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-bar-container { flex: 1; height: 24px; background: #eee; border-radius: 4px; position: relative; overflow: hidden; }
  .blog-fullhtml .chart-bar { height: 100%; border-radius: 4px; display: flex; align-items: center; justify-content: flex-end; padding-right: 8px; font-size: 0.7em; font-weight: 700; color: white; }
  .blog-fullhtml .chart-bar.small-text { justify-content: flex-start; padding-left: 8px; color: #333; }
  .blog-fullhtml .difficulty-section { margin-bottom: 15px; }
  .blog-fullhtml .difficulty-header { font-weight: 700; font-size: 0.85em; color: #555; margin: 12px 0 6px 0; padding: 3px 8px; background: #f0f0f0; border-radius: 4px; display: inline-block; font-family: -apple-system, sans-serif; }
  .blog-fullhtml .chart-title { text-align: center; margin-bottom: 15px; }
  .blog-fullhtml .chart-title h4 { font-size: 1em; color: #333; margin: 0; font-style: normal; }
  .blog-fullhtml .chart-title p { font-size: 0.75em; color: #888; margin: 3px 0 0; }
  .blog-fullhtml .chart-caption { font-size: 0.75em; color: #777; margin-top: 12px; text-align: center; font-style: italic; }
  .blog-fullhtml .chart-divider { border-top: 1px solid #ddd; margin: 6px 0 6px 140px; }
  .blog-fullhtml .paper-link { margin: 56px 0 0; padding: 28px; background: linear-gradient(135deg, #004d40, #00897b); border-radius: 10px; color: #e0e8f0; }
  .blog-fullhtml .paper-link p { font-size: 0.95rem; color: #b2dfdb; margin-bottom: 8px; }
  .blog-fullhtml .paper-link p:last-child { margin-bottom: 0; }
  .blog-fullhtml .paper-link a { color: #80cbc4; border-bottom-color: rgba(128,203,196,0.4); }
  .blog-fullhtml .paper-link a:hover { color: #b2dfdb; border-bottom-color: #b2dfdb; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: -apple-system, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }
  /* Interactive Graph Widget */
  .blog-fullhtml .ig-widget { max-width: 960px; width: 100%; margin: 25px auto; font-family: -apple-system, sans-serif; position: relative; box-sizing: border-box; }
  .blog-fullhtml .ig-widget-title { text-align: center; color: #333; margin-bottom: 4px; font-size: 1.1em; font-weight: 700; }
  .blog-fullhtml .ig-widget-sub { text-align: center; color: #777; font-size: 0.85em; margin-bottom: 14px; }
  .blog-fullhtml .ig-layout { display: flex; gap: 12px; align-items: stretch; justify-content: center; flex-wrap: nowrap; }
  .blog-fullhtml .ig-states { background: #fafafa; border-radius: 10px; padding: 16px 14px; box-shadow: 0 2px 8px rgba(0,0,0,0.06); display: flex; flex-direction: column; align-items: center; width: 175px; flex-shrink: 0; }
  .blog-fullhtml .ig-states h4 { font-size: 0.75em; color: #888; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 8px; }
  .blog-fullhtml .ig-graph { background: #fafafa; border-radius: 10px; padding: 16px; box-shadow: 0 2px 8px rgba(0,0,0,0.06); flex: 1; min-width: 480px; }
  .blog-fullhtml .ig-graph h4 { font-size: 0.75em; color: #888; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 6px; text-align: center; }
  .blog-fullhtml .ig-arrow { font-size: 1.6em; color: #ccc; align-self: center; padding: 0 2px; }
  .blog-fullhtml .ig-block { cursor: pointer; }
  .blog-fullhtml .ig-block rect { transition: stroke-width 0.2s, filter 0.2s; }
  .blog-fullhtml .ig-node { cursor: pointer; }
  .blog-fullhtml .ig-node circle, .blog-fullhtml .ig-node rect { transition: stroke-width 0.2s, filter 0.2s, opacity 0.3s; }
  .blog-fullhtml .ig-edge { transition: stroke-width 0.3s, opacity 0.3s; }
  .blog-fullhtml .ig-dimmed { opacity: 0.12 !important; }
  .blog-fullhtml .ig-highlighted circle, .blog-fullhtml .ig-highlighted rect { stroke-width: 3.5px !important; filter: drop-shadow(0 0 5px rgba(0,0,0,0.25)); }
  .blog-fullhtml .ig-highlighted-block rect { stroke-width: 3px !important; filter: drop-shadow(0 0 6px rgba(0,0,0,0.3)); }
  .blog-fullhtml .ig-edge.ig-highlighted { stroke-width: 3.5px !important; opacity: 1 !important; }
  .blog-fullhtml .ig-tooltip { position: fixed; background: #333; color: white; padding: 8px 14px; border-radius: 6px; font-size: 0.8em; pointer-events: none; opacity: 0; transition: opacity 0.15s; z-index: 10000; max-width: 260px; line-height: 1.4; }
  .blog-fullhtml .ig-tooltip.visible { opacity: 1; }
  .blog-fullhtml .ig-legend { display: flex; flex-wrap: wrap; justify-content: center; margin-top: 10px; gap: 4px 16px; max-width: 420px; margin-left: auto; margin-right: auto; }
  .blog-fullhtml .ig-legend-item { display: flex; align-items: center; gap: 5px; font-size: 0.75em; color: #666; }
  .blog-fullhtml .ig-legend-dot { width: 11px; height: 11px; border-radius: 50%; }
  .blog-fullhtml .ig-legend-line { width: 16px; height: 3px; border-radius: 2px; }
  .blog-fullhtml .ig-instructions { text-align: center; color: #999; font-size: 0.78em; margin-top: 8px; font-style: italic; }
  @media (max-width: 700px) {
      .blog-fullhtml .hero h1 { font-size: 1.7rem; }
      .blog-fullhtml .blog-container { padding: 32px 16px 60px; }
      .blog-fullhtml .ig-layout { flex-direction: column; align-items: center; flex-wrap: wrap; }
      .blog-fullhtml .ig-states { width: 100%; max-width: 250px; }
      .blog-fullhtml .ig-graph { min-width: unset; width: 100%; }
      .blog-fullhtml .ig-arrow { transform: rotate(90deg); }
  }
  /* ============================== */
  /* === DARK MODE === */
  /* ============================== */
  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #80cbc4; border-bottom-color: #26a69a; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #a7d8d2; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml a { color: #80cbc4; border-bottom-color: rgba(128,203,196,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #b2dfdb; border-bottom-color: #b2dfdb; }
  html[data-theme="dark"] .blog-fullhtml p { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml li { color: #c0c8d0; }
  html[data-theme="dark"] .blog-fullhtml blockquote { background: #1a2a28; border-left-color: #26a69a; color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #80cbc4; }
  html[data-theme="dark"] .blog-fullhtml .equation-note { background: #1a2035; border-color: #3a4a5c; color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .equation-note::before { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .results-chart-container { background: #1e2530; }
  html[data-theme="dark"] .blog-fullhtml .chart-label { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .chart-bar-container { background: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .chart-bar.small-text { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .chart-title h4 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .chart-title p { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .chart-caption { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .chart-divider { border-top-color: #2a3545; }
  html[data-theme="dark"] .blog-fullhtml .difficulty-header { background: #2a3545; color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #6a7888; border-top-color: #2a3545; }
  /* Dark mode: interactive widget */
  html[data-theme="dark"] .blog-fullhtml .ig-widget-title { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml .ig-widget-sub { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .ig-states { background: #1e2530; box-shadow: 0 2px 8px rgba(0,0,0,0.2); }
  html[data-theme="dark"] .blog-fullhtml .ig-states h4 { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .ig-graph { background: #1e2530; box-shadow: 0 2px 8px rgba(0,0,0,0.2); }
  html[data-theme="dark"] .blog-fullhtml .ig-graph h4 { color: #8899aa; }
  html[data-theme="dark"] .blog-fullhtml .ig-arrow { color: #556677; }
  html[data-theme="dark"] .blog-fullhtml .ig-legend-item { color: #9aa8b8; }
  html[data-theme="dark"] .blog-fullhtml .ig-instructions { color: #6a7888; }
  html[data-theme="dark"] .blog-fullhtml .ig-tooltip { background: #e0e8f0; color: #1a1a2e; }
  /* Dark mode: SVG text */
  html[data-theme="dark"] .blog-fullhtml svg text[fill="#333"] { fill: #c0c8d0; }
  html[data-theme="dark"] .blog-fullhtml svg text[fill="#5d4037"] { fill: #c0c8d0; }
  html[data-theme="dark"] .blog-fullhtml svg text[fill="#aaa"] { fill: #8899aa; }
---

<header class="hero">
    <div class="series-label">Research Deep Dive • NeurIPS 2025</div>
    <h1>You Don't Need to Rank All States. Just Rank the Actions in Front of You.</h1>
    <p class="subtitle">How a simple shift in learning objective—from global value functions to local action ranking—yields planning policies that generalize 8x beyond training size. A deep dive into GABAR.</p>
</header>

<article class="blog-container">

<h2>The Setup: Planning is Hard, and It Gets Harder Fast</h2>

<p>Imagine you're organizing a warehouse. You have 10 packages, a few trucks, and a couple of airplanes. A classical planner can figure out the optimal delivery sequence in seconds. Now scale that to 30 packages across multiple cities. The same planner might run for hours—or never finish at all.</p>

<p>This is the fundamental scaling challenge in classical AI planning. The state space grows exponentially with the number of objects. Planning is NP-hard in most domains. Traditional planners use heuristic search, which works brilliantly on small problems but chokes on large ones.</p>

<p>The natural question: <strong>can we learn planning strategies from small, solvable problems and apply them to large, unsolvable ones?</strong></p>

<p>This is exactly what our paper <em>Graph Neural Network Based Action Ranking for Planning</em> addresses. We present <strong>GABAR</strong>—a system that trains on problems with 6-10 objects and successfully solves problems with 100+ objects, achieving 89% success rate on instances 8x larger than anything it saw during training.</p>

<hr>

<h2>The Core Insight: Stop Trying to Learn the Hardest Thing</h2>

<p>Most learning-based planning approaches try to learn a <strong>value function</strong> V(s)—an estimate of how far state s is from the goal. The idea is simple: if you know V(s) for every state, you can greedily pick the action that leads to the state with the lowest value.</p>

<p>The problem? This requires <strong>global consistency</strong>. Your value function must correctly rank <em>every reachable state</em> relative to <em>every other reachable state</em>. As the problem grows, the number of states explodes exponentially.</p>

<p>Here's our key realization:</p>

<blockquote>You don't need to rank all states. You only need to rank the actions available <em>right now</em>.</blockquote>

<p>At any given state in a typical planning problem, you might have 5-50 applicable actions. Ranking these is a <em>local</em> problem. You don't need to know anything about states three steps ahead. You just need to identify which of the currently available actions is most promising.</p>

<p>This is a fundamentally simpler learning target:</p>
<ul>
    <li><strong>Value function learning</strong>: Learn a function consistent across millions of states</li>
    <li><strong>Action ranking</strong>: Learn to pick the best among a handful of local options</li>
</ul>

<hr>

<h2>How GABAR Works: Three Ideas Working Together</h2>

<h3>1. Action-Centric Graph Representation</h3>

<p>Most GNN-based planners represent a state as a graph of objects connected by predicates. We add something new: <strong>action nodes</strong>. Every applicable action in the current state gets its own node in the graph, connected to the objects it involves.</p>

<p>Why does this matter? Consider the action <code>unstack(A, B)</code>—pick up block A from block B. In our graph, this action has explicit edges to both A and B, with edge features encoding which parameter position each object fills and which predicates each object satisfies.</p>

<!-- Interactive Graph Widget -->
<div class="ig-widget" id="ig-widget-main">
    <p class="ig-widget-title">Interactive: How GABAR Converts a Planning State to a Graph</p>
    <p class="ig-widget-sub">Hover over blocks in start/goal state to see corresponding graph elements.</p>
    <div class="ig-layout">
        <div class="ig-states">
            <h4>Start State</h4>
            <svg width="150" height="140" viewBox="0 0 150 140">
                <rect x="10" y="130" width="130" height="5" rx="2" fill="#4a90d9" opacity="0.4"/>
                <g class="ig-block" data-hover="start-O2"><rect x="25" y="85" width="52" height="40" rx="7" fill="#fff9c4" stroke="#f9a825" stroke-width="2"/><text x="51" y="110" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O2</text></g>
                <g class="ig-block" data-hover="start-O3"><rect x="25" y="42" width="52" height="40" rx="7" fill="#fff9c4" stroke="#f9a825" stroke-width="2"/><text x="51" y="67" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O3</text></g>
                <g class="ig-block" data-hover="start-O1"><rect x="90" y="85" width="52" height="40" rx="7" fill="#fff9c4" stroke="#f9a825" stroke-width="2"/><text x="116" y="110" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O1</text></g>
            </svg>
            <h4 style="margin-top:8px;">Goal State</h4>
            <svg width="150" height="160" viewBox="0 0 150 160">
                <rect x="10" y="148" width="130" height="5" rx="2" fill="#4a90d9" opacity="0.4"/>
                <g class="ig-block" data-hover="goal-O1"><rect x="47" y="105" width="52" height="40" rx="7" fill="#fff9c4" stroke="#f9a825" stroke-width="2" stroke-dasharray="5,3"/><text x="73" y="130" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O1</text></g>
                <g class="ig-block" data-hover="goal-O2"><rect x="47" y="63" width="52" height="40" rx="7" fill="#fff9c4" stroke="#f9a825" stroke-width="2" stroke-dasharray="5,3"/><text x="73" y="88" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O2</text></g>
                <g class="ig-block" data-hover="goal-O3"><rect x="47" y="21" width="52" height="40" rx="7" fill="#fff9c4" stroke="#f9a825" stroke-width="2" stroke-dasharray="5,3"/><text x="73" y="46" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O3</text></g>
                <text x="75" y="158" text-anchor="middle" font-size="8.5" fill="#aaa">(dashed = goal)</text>
            </svg>
        </div>
        <div class="ig-arrow">→</div>
        <div class="ig-graph">
            <h4>Graph Representation</h4>
            <svg width="100%" viewBox="0 0 570 290" style="max-height:290px;">
                <line class="ig-edge" data-edge="action" data-connects="pickup O1" x1="390" y1="55" x2="390" y2="120" stroke="#4a90d9" stroke-width="2.5" opacity="0.6"/>
                <line class="ig-edge" data-edge="action" data-connects="unstack O3" x1="130" y1="55" x2="130" y2="120" stroke="#4a90d9" stroke-width="2.5" opacity="0.6"/>
                <line class="ig-edge" data-edge="action" data-connects="unstack O2" x1="130" y1="55" x2="260" y2="120" stroke="#4a90d9" stroke-width="2.5" opacity="0.6"/>
                <line class="ig-edge" data-edge="current-pred" data-connects="on_O3O2 O3" x1="95" y1="220" x2="130" y2="170" stroke="#e57373" stroke-width="2" opacity="0.5" stroke-dasharray="5,3"/>
                <line class="ig-edge" data-edge="current-pred" data-connects="on_O3O2 O2" x1="95" y1="220" x2="260" y2="170" stroke="#e57373" stroke-width="2" opacity="0.5" stroke-dasharray="5,3"/>
                <line class="ig-edge" data-edge="current-pred" data-connects="clear_O3 O3" x1="225" y1="220" x2="130" y2="170" stroke="#e57373" stroke-width="2" opacity="0.5" stroke-dasharray="5,3"/>
                <line class="ig-edge" data-edge="goal-pred" data-connects="g_on_O2O1 O2" x1="355" y1="220" x2="260" y2="170" stroke="#4caf50" stroke-width="2" opacity="0.5" stroke-dasharray="5,3"/>
                <line class="ig-edge" data-edge="goal-pred" data-connects="g_on_O2O1 O1" x1="355" y1="220" x2="390" y2="170" stroke="#4caf50" stroke-width="2" opacity="0.5" stroke-dasharray="5,3"/>
                <line class="ig-edge" data-edge="goal-pred" data-connects="g_on_O3O2 O3" x1="470" y1="220" x2="130" y2="170" stroke="#4caf50" stroke-width="2" opacity="0.5" stroke-dasharray="5,3"/>
                <line class="ig-edge" data-edge="goal-pred" data-connects="g_on_O3O2 O2" x1="470" y1="220" x2="260" y2="170" stroke="#4caf50" stroke-width="2" opacity="0.5" stroke-dasharray="5,3"/>
                <g class="ig-node" data-ntype="action" data-id="unstack" data-tip="Unstack(O3,O2): removes O3 from on top of O2."><rect x="60" y="22" width="140" height="38" rx="6" fill="#4a90d9" stroke="#2d6eb5" stroke-width="2"/><text x="130" y="46" text-anchor="middle" font-size="12" fill="white" font-weight="700">Unstack(O3, O2)</text></g>
                <g class="ig-node" data-ntype="action" data-id="pickup" data-tip="PickUp(O1): picks up O1 from the table."><rect x="325" y="22" width="130" height="38" rx="6" fill="#4a90d9" stroke="#2d6eb5" stroke-width="2"/><text x="390" y="46" text-anchor="middle" font-size="12" fill="white" font-weight="700">PickUp(O1)</text></g>
                <g class="ig-node" data-ntype="object" data-id="O3" data-tip="Object O3: clear, on top of O2."><circle cx="130" cy="145" r="24" fill="#fff9c4" stroke="#f9a825" stroke-width="2.5"/><text x="130" y="150" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O3</text></g>
                <g class="ig-node" data-ntype="object" data-id="O2" data-tip="Object O2: has O3 on top."><circle cx="260" cy="145" r="24" fill="#fff9c4" stroke="#f9a825" stroke-width="2.5"/><text x="260" y="150" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O2</text></g>
                <g class="ig-node" data-ntype="object" data-id="O1" data-tip="Object O1: clear, on table."><circle cx="390" cy="145" r="24" fill="#fff9c4" stroke="#f9a825" stroke-width="2.5"/><text x="390" y="150" text-anchor="middle" font-size="13" font-weight="700" fill="#5d4037">O1</text></g>
                <g class="ig-node" data-ntype="current-pred" data-id="on_O3O2" data-tip="On(O3,O2): O3 is currently on top of O2."><rect x="40" y="218" width="110" height="30" rx="15" fill="#e57373" stroke="#c62828" stroke-width="1.5"/><text x="95" y="237" text-anchor="middle" font-size="10.5" fill="white" font-weight="600">On(O3, O2)</text></g>
                <g class="ig-node" data-ntype="current-pred" data-id="clear_O3" data-tip="Clear(O3): O3 has nothing on top of it."><rect x="165" y="218" width="110" height="30" rx="15" fill="#e57373" stroke="#c62828" stroke-width="1.5"/><text x="220" y="237" text-anchor="middle" font-size="10.5" fill="white" font-weight="600">Clear(O3)</text></g>
                <g class="ig-node" data-ntype="goal-pred" data-id="g_on_O2O1" data-tip="Goal: On(O2,O1) — O2 needs to be placed on O1."><rect x="290" y="218" width="130" height="30" rx="15" fill="#4caf50" stroke="#2e7d32" stroke-width="1.5"/><text x="355" y="237" text-anchor="middle" font-size="10.5" fill="white" font-weight="600">Goal: On(O2, O1)</text></g>
                <g class="ig-node" data-ntype="goal-pred" data-id="g_on_O3O2" data-tip="Goal: On(O3,O2) — O3 should remain on O2."><rect x="430" y="218" width="130" height="30" rx="15" fill="#4caf50" stroke="#2e7d32" stroke-width="1.5"/><text x="495" y="237" text-anchor="middle" font-size="10.5" fill="white" font-weight="600">Goal: On(O3, O2)</text></g>
            </svg>
            <div class="ig-legend">
                <div class="ig-legend-item"><div class="ig-legend-dot" style="background:#4a90d9;"></div>Action node</div>
                <div class="ig-legend-item"><div class="ig-legend-dot" style="background:#fff9c4;border:2px solid #f9a825;"></div>Object node</div>
                <div class="ig-legend-item"><div class="ig-legend-dot" style="background:#e57373;border-radius:3px;"></div>Current predicate</div>
                <div class="ig-legend-item"><div class="ig-legend-dot" style="background:#4caf50;border-radius:3px;"></div>Goal predicate</div>
            </div>
        </div>
    </div>
    <p class="ig-instructions">Hover over blocks (left) or graph nodes (right) to see connections.</p>
</div>
<div class="ig-tooltip" id="ig-tooltip-main"></div>

<h3>2. GNN Encoder with Global Context</h3>

<p>Our GNN processes the graph through 9 rounds of message passing. Each round updates edges, then nodes, then a global summary vector. The <strong>global node</strong> is crucial—it acts as a communication shortcut so information from distant parts of the graph can reach each other regardless of graph size.</p>

<h3>3. Conditional GRU Decoder</h3>

<p>Actions have structure—a schema and ordered parameters with dependencies. Our decoder uses a GRU that builds actions sequentially: select an action schema, then select each parameter conditioned on all previous selections. Each selection is conditioned through the GRU's hidden state, with beam search (width 2) to maintain multiple candidates.</p>

<hr>

<h2>The Ablation Story: What Matters Most</h2>

<div class="results-chart-container">
    <div class="chart-title">
        <h4>Ablation Study: Coverage on Hard Problems</h4>
        <p>Each bar shows coverage when one component is removed</p>
    </div>
    <div class="chart">
        <div class="chart-row"><div class="chart-label">Full GABAR</div><div class="chart-bar-container"><div class="chart-bar" style="width: 89.2%; background: #00A3A1;">89.2%</div></div></div>
        <div class="chart-divider" style="margin-left:0"></div>
        <div class="chart-row"><div class="chart-label" style="color:#c0392b;">- Cond. Decoder</div><div class="chart-bar-container"><div class="chart-bar" style="width: 60%; background: #e67e22;">60.0%</div></div></div>
        <div class="chart-row"><div class="chart-label" style="color:#c0392b;">- Global Node</div><div class="chart-bar-container"><div class="chart-bar" style="width: 42.5%; background: #e74c3c;">42.5%</div></div></div>
        <div class="chart-row"><div class="chart-label" style="color:#c0392b;">- Ranking Obj.</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 12.1%; background: #c0392b;">12.1%</div></div></div>
        <div class="chart-row"><div class="chart-label" style="color:#c0392b;">- Action Nodes</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 7.4%; background: #922b21;">7.4%</div></div></div>
    </div>
    <p class="chart-caption">Removing action nodes or the ranking objective causes near-total failure.</p>
</div>

<p>The two most critical components are both about <em>what information is available</em>: the action-centric graph (82-point impact) and the ranking objective (77-point impact). The decoder and global node are about <em>how that information is processed</em>—important but secondary.</p>

<hr>

<h2>Results: Generalization That Actually Works</h2>

<div class="results-chart-container">
    <div class="chart-title">
        <h4>Coverage (% Problems Solved) by Difficulty</h4>
        <p>Averaged across 8 planning domains</p>
    </div>
    <div class="difficulty-section">
        <div class="difficulty-header">Easy Problems</div>
        <div class="chart">
            <div class="chart-row"><div class="chart-label">GABAR (ours)</div><div class="chart-bar-container"><div class="chart-bar" style="width: 95.5%; background: #00A3A1;">95.5%</div></div></div>
            <div class="chart-row"><div class="chart-label">GPL</div><div class="chart-bar-container"><div class="chart-bar" style="width: 79.1%; background: #7ab8b6;">79.1%</div></div></div>
            <div class="chart-row"><div class="chart-label">ASNets</div><div class="chart-bar-container"><div class="chart-bar" style="width: 76%; background: #9ec5c4;">76.0%</div></div></div>
            <div class="chart-row"><div class="chart-label">Gemini 2.5 Pro</div><div class="chart-bar-container"><div class="chart-bar" style="width: 44%; background: #d4a0e5;">44.0%</div></div></div>
            <div class="chart-row"><div class="chart-label">OpenAI O3</div><div class="chart-bar-container"><div class="chart-bar" style="width: 33.4%; background: #c490d8;">33.4%</div></div></div>
        </div>
    </div>
    <div class="difficulty-section">
        <div class="difficulty-header">Medium Problems</div>
        <div class="chart">
            <div class="chart-row"><div class="chart-label">GABAR (ours)</div><div class="chart-bar-container"><div class="chart-bar" style="width: 92.2%; background: #00A3A1;">92.2%</div></div></div>
            <div class="chart-row"><div class="chart-label">GPL</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 28.5%; background: #7ab8b6;">28.5%</div></div></div>
            <div class="chart-row"><div class="chart-label">ASNets</div><div class="chart-bar-container"><div class="chart-bar" style="width: 65.4%; background: #9ec5c4;">65.4%</div></div></div>
            <div class="chart-row"><div class="chart-label">Gemini 2.5 Pro</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 17.1%; background: #d4a0e5;">17.1%</div></div></div>
            <div class="chart-row"><div class="chart-label">OpenAI O3</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 11.6%; background: #c490d8;">11.6%</div></div></div>
        </div>
    </div>
    <div class="difficulty-section">
        <div class="difficulty-header">Hard Problems (8x training size)</div>
        <div class="chart">
            <div class="chart-row"><div class="chart-label">GABAR (ours)</div><div class="chart-bar-container"><div class="chart-bar" style="width: 89.2%; background: #00A3A1;">89.2%</div></div></div>
            <div class="chart-row"><div class="chart-label">GPL</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 6.5%; background: #7ab8b6;">6.5%</div></div></div>
            <div class="chart-row"><div class="chart-label">ASNets</div><div class="chart-bar-container"><div class="chart-bar" style="width: 48.5%; background: #9ec5c4;">48.5%</div></div></div>
            <div class="chart-row"><div class="chart-label">Gemini 2.5 Pro</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 3%; background: #d4a0e5;">1.5%</div></div></div>
            <div class="chart-row"><div class="chart-label">OpenAI O3</div><div class="chart-bar-container"><div class="chart-bar small-text" style="width: 2%; background: #c490d8;">0.4%</div></div></div>
        </div>
    </div>
    <p class="chart-caption">GABAR maintains ~89% coverage on hard problems while baselines and LLMs collapse below 50%.</p>
</div>

<p>The coverage drop from easy to hard for GABAR is minimal: 95.5% → 89.2%. Compare this to GPL (79% → 6.5%) or state-of-the-art LLMs (33-44% → 0.4-1.5%).</p>

<p>On Blocks World, Gripper, and Miconic, GABAR achieves <strong>100% success rate at all difficulty levels</strong>—solving 40-block, 100-ball, and 100-passenger problems after training on instances with fewer than 10 objects.</p>

<h3>Plan Quality, Not Just Coverage</h3>

<p>GABAR's Plan Quality Ratio stays at approximately 1.0 across all difficulties (Easy: 1.04, Medium: 1.01, Hard: 0.99), meaning GABAR's plans are comparable in length to those from a state-of-the-art satisficing planner.</p>

<hr>

<h2>The Deeper Lessons</h2>

<h3>1. Local Objectives Can Beat Global Ones</h3>
<p>If your downstream task only requires <em>local decisions</em>, formulate your learning objective locally. A local ranking function needs consistency only within each decision point's option set—a strictly easier learning problem.</p>

<h3>2. Represent What You're Deciding About</h3>
<p>Your input representation should explicitly encode the entities you're making decisions about. If you want to rank actions, give the network direct access to action structure.</p>

<h3>3. Structure Your Decoder to Match Your Output Structure</h3>
<p>If your output has compositional structure, decode it compositionally. This is why our conditional decoder works for planning actions with inter-parameter dependencies.</p>

<h3>4. Global Context Nodes Enable Fixed-Depth Architectures to Scale</h3>
<p>The global node lets a fixed-depth GNN (9 layers) process arbitrarily large graphs by providing a "shortcut" for information flow—the graph equivalent of the [CLS] token in transformers.</p>

<h3>5. Train on Easy, Deploy on Hard</h3>
<p>GABAR trains exclusively on problems trivial for classical planners. This works because the patterns are compositional and the architecture has appropriate inductive biases.</p>

<hr>

<h2>Technical Details</h2>

<ul>
    <li><strong>Training</strong>: Adam optimizer, lr = 0.0005, batch size 16, hidden dim 64</li>
    <li><strong>Architecture</strong>: 9 GNN rounds, beam width 2, attention-based aggregation</li>
    <li><strong>Data</strong>: ~3,000-7,000 training examples per domain, generated by solving random small PDDL instances</li>
    <li><strong>Training time</strong>: 1-2 hours per domain on a single RTX 3080</li>
    <li><strong>Evaluation</strong>: 8 standard planning benchmarks (Blocks, Gripper, Miconic, Spanner, Logistics, Rovers, Visitall, Grid)</li>
</ul>

<hr>

<h2>Summary</h2>

<p>GABAR demonstrates that a simple conceptual shift—from global value functions to local action ranking—combined with the right structural inductive biases, enables learned planning policies that genuinely generalize. The system trains on toy problems and solves real ones, maintains high plan quality as it scales, and substantially outperforms both classical learning baselines and state-of-the-art LLMs.</p>

<p>The broader takeaway: when you're building a system that needs to generalize, ask yourself—am I trying to learn something harder than I need to? Can I reformulate my objective to be <em>local</em> rather than <em>global</em>? Can I represent my decision space <em>explicitly</em> rather than <em>implicitly</em>?</p>

<p>If the answer to any of these is yes, you might be working harder than necessary.</p>

<div class="paper-link">
    <p><em>This work was presented at NeurIPS 2025. Paper, code, and project page available at the project website.</em></p>
    <p><em>Supported by the Army Research Office under grant W911NF2210251.</em></p>
</div>

</article>

<div class="blog-footer">
    <p>GABAR — NeurIPS 2025 • Research Deep Dive</p>
</div>

<script>
(function() {
    var root = document.getElementById('ig-widget-main');
    var tooltip = document.getElementById('ig-tooltip-main');
    if (!root || !tooltip) return;
    var allNodes = root.querySelectorAll('.ig-node');
    var allEdges = root.querySelectorAll('.ig-edge');
    var allBlocks = root.querySelectorAll('.ig-block');
    var startMap = {
        'start-O1': { objects: ['O1'], actions: ['pickup'], currentPreds: [], goalPreds: [] },
        'start-O2': { objects: ['O2'], actions: ['unstack'], currentPreds: ['on_O3O2'], goalPreds: [] },
        'start-O3': { objects: ['O3'], actions: ['unstack'], currentPreds: ['on_O3O2', 'clear_O3'], goalPreds: [] }
    };
    var goalMap = {
        'goal-O1': { objects: ['O1'], actions: [], currentPreds: [], goalPreds: ['g_on_O2O1'] },
        'goal-O2': { objects: ['O2'], actions: [], currentPreds: [], goalPreds: ['g_on_O2O1', 'g_on_O3O2'] },
        'goal-O3': { objects: ['O3'], actions: [], currentPreds: [], goalPreds: ['g_on_O3O2'] }
    };
    var nodeMap = {
        'pickup': { objects: ['O1'], actions: ['pickup'], currentPreds: [], goalPreds: [] },
        'unstack': { objects: ['O2', 'O3'], actions: ['unstack'], currentPreds: [], goalPreds: [] },
        'O1': { objects: ['O1'], actions: ['pickup'], currentPreds: [], goalPreds: ['g_on_O2O1'] },
        'O2': { objects: ['O2'], actions: ['unstack'], currentPreds: ['on_O3O2'], goalPreds: ['g_on_O2O1', 'g_on_O3O2'] },
        'O3': { objects: ['O3'], actions: ['unstack'], currentPreds: ['on_O3O2', 'clear_O3'], goalPreds: ['g_on_O3O2'] },
        'clear_O3': { objects: ['O3'], actions: [], currentPreds: ['clear_O3'], goalPreds: [] },
        'on_O3O2': { objects: ['O2', 'O3'], actions: [], currentPreds: ['on_O3O2'], goalPreds: [] },
        'g_on_O2O1': { objects: ['O1', 'O2'], actions: [], currentPreds: [], goalPreds: ['g_on_O2O1'] },
        'g_on_O3O2': { objects: ['O2', 'O3'], actions: [], currentPreds: [], goalPreds: ['g_on_O3O2'] }
    };
    function showTip(e, t) { tooltip.textContent = t; tooltip.classList.add('visible'); moveTip(e); }
    function moveTip(e) { tooltip.style.left = (e.clientX + 12) + 'px'; tooltip.style.top = (e.clientY - 8) + 'px'; }
    function hideTip() { tooltip.classList.remove('visible'); }
    function highlight(m) {
        allNodes.forEach(function(n) { n.classList.add('ig-dimmed'); });
        allEdges.forEach(function(e) { e.classList.add('ig-dimmed'); });
        allBlocks.forEach(function(b) { b.classList.add('ig-dimmed'); });
        m.objects.forEach(function(id) {
            root.querySelectorAll('.ig-node[data-id="'+id+'"]').forEach(function(n) { n.classList.remove('ig-dimmed'); n.classList.add('ig-highlighted'); });
            root.querySelectorAll('.ig-block[data-hover*="'+id+'"]').forEach(function(b) { b.classList.remove('ig-dimmed'); b.classList.add('ig-highlighted-block'); });
        });
        m.actions.forEach(function(id) { root.querySelectorAll('.ig-node[data-id="'+id+'"]').forEach(function(n) { n.classList.remove('ig-dimmed'); n.classList.add('ig-highlighted'); }); });
        m.currentPreds.forEach(function(id) { root.querySelectorAll('.ig-node[data-id="'+id+'"]').forEach(function(n) { n.classList.remove('ig-dimmed'); n.classList.add('ig-highlighted'); }); });
        m.goalPreds.forEach(function(id) { root.querySelectorAll('.ig-node[data-id="'+id+'"]').forEach(function(n) { n.classList.remove('ig-dimmed'); n.classList.add('ig-highlighted'); }); });
        allEdges.forEach(function(edge) {
            var c = edge.dataset.connects.split(' '), et = edge.dataset.edge, h = false;
            if (et === 'action' && m.actions.indexOf(c[0]) >= 0 && m.objects.indexOf(c[1]) >= 0) h = true;
            if (et === 'current-pred' && m.currentPreds.indexOf(c[0]) >= 0 && m.objects.indexOf(c[1]) >= 0) h = true;
            if (et === 'goal-pred' && m.goalPreds.indexOf(c[0]) >= 0 && m.objects.indexOf(c[1]) >= 0) h = true;
            if (h) { edge.classList.remove('ig-dimmed'); edge.classList.add('ig-highlighted'); }
        });
    }
    function clearAll() {
        allNodes.forEach(function(n) { n.classList.remove('ig-dimmed', 'ig-highlighted'); });
        allEdges.forEach(function(e) { e.classList.remove('ig-dimmed', 'ig-highlighted'); });
        allBlocks.forEach(function(b) { b.classList.remove('ig-dimmed', 'ig-highlighted-block'); });
    }
    allBlocks.forEach(function(block) {
        var key = block.dataset.hover;
        block.addEventListener('mouseenter', function(e) {
            var m = startMap[key] || goalMap[key]; if (m) highlight(m);
            var obj = key.split('-')[1];
            showTip(e, key.startsWith('goal') ? 'Goal: '+obj+' — see goal predicates (green)' : 'Current: '+obj+' — see actions & current predicates');
        });
        block.addEventListener('mousemove', moveTip);
        block.addEventListener('mouseleave', function() { clearAll(); hideTip(); });
    });
    allNodes.forEach(function(node) {
        var id = node.dataset.id;
        node.addEventListener('mouseenter', function(e) { var m = nodeMap[id]; if (m) highlight(m); showTip(e, node.dataset.tip); });
        node.addEventListener('mousemove', moveTip);
        node.addEventListener('mouseleave', function() { clearAll(); hideTip(); });
    });
})();
</script>
