---
layout: fullhtml-post
title: "What to Learn — A Survey of Learning Objectives"
date: 2026-04-30
categories: ["Learning for Planning"]
tags: ["planning", "gnn", "survey"]
description: "Three families of methods, each picking a different target for the model to predict. The choice of target ends up mattering more than the choice of architecture. Part 2 of the Learning for Planning series."
_styles: >
  .blog-fullhtml {
  font-family: 'Charter', 'Georgia', serif;
  line-height: 1.75;
  color: #1a1a1a;
  font-size: 18px;
  }
  .blog-fullhtml h1 { font-size: 2em; line-height: 1.2; margin-top: 1.2em; }
  .blog-fullhtml h2 { font-size: 1.5em; margin-top: 2em; color: #222; }
  .blog-fullhtml h3 { font-size: 1.2em; margin-top: 1.5em; }
  .blog-fullhtml .subtitle { font-size: 1.05em; color: #555; font-style: italic; margin-top: -0.5em; }
  .blog-fullhtml hr { border: none; border-top: 1px solid #ddd; margin: 2em 0; }
  .blog-fullhtml code { background: #f4f4f4; padding: 2px 5px; border-radius: 3px; font-size: 0.9em; }
  .blog-fullhtml em { color: #333; }
  .blog-fullhtml blockquote { border-left: 3px solid #00A3A1; margin: 1.5em 0; padding: 0.5em 1.5em; color: #444; background: #f0fafa; }
  .blog-fullhtml .paper-tag { display: inline-block; font-family: 'JetBrains Mono', monospace; font-size: 0.78em; background: #2D2044; color: #C5A55A; padding: 1px 8px; border-radius: 3px; font-weight: 600; margin-right: 4px; }

  .blog-fullhtml .series-nav { background: #e0f5f5; border-left: 4px solid #00A3A1; border-radius: 0 8px 8px 0; padding: 14px 18px; margin: 0 0 24px; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; font-size: 0.92em; }
  .blog-fullhtml .series-nav strong { color: #00807E; }
  .blog-fullhtml .series-nav .series-nav-links { margin-top: 6px; font-size: 0.85em; color: #555; }
  .blog-fullhtml .series-nav a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }
  .blog-fullhtml .series-nav a:hover { color: #005856; }

  .blog-fullhtml .series-footer { margin: 3em 0 2em; padding: 20px 22px; background: #f0fafa; border: 1px solid #c2e4e3; border-radius: 10px; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .series-footer strong { color: #00807E; }
  .blog-fullhtml .series-footer p { font-size: 0.9em; color: #444; margin: 8px 0 0; line-height: 1.6; }
  .blog-fullhtml .series-footer a { color: #00807E; text-decoration: none; border-bottom: 1px solid #00A3A155; }

  .blog-fullhtml .family-callout { margin: 28px 0 18px; padding: 16px 20px; border-radius: 10px; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .family-callout.heuristic { background: #fef5e7; border-left: 4px solid #d4a017; }
  .blog-fullhtml .family-callout.value { background: #ecedfa; border-left: 4px solid #5b6abf; }
  .blog-fullhtml .family-callout.ranking { background: #e0f5f5; border-left: 4px solid #00A3A1; }
  .blog-fullhtml .family-callout .label { font-size: 0.7rem; text-transform: uppercase; letter-spacing: 2px; font-weight: 700; margin-bottom: 4px; }
  .blog-fullhtml .family-callout.heuristic .label { color: #8b6914; }
  .blog-fullhtml .family-callout.value .label { color: #3d4a9e; }
  .blog-fullhtml .family-callout.ranking .label { color: #00807E; }
  .blog-fullhtml .family-callout h3 { margin: 0 0 6px; font-family: 'Playfair Display', Georgia, serif; font-size: 1.1rem; color: #2D2044; }
  .blog-fullhtml .family-callout p { font-size: 0.92em; color: #333; line-height: 1.5; margin: 0; }

  .blog-fullhtml .insight { margin: 24px 0; padding: 14px 18px; background: #2D2044; border-radius: 8px; color: #fff; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .insight .label { font-size: 0.68rem; text-transform: uppercase; letter-spacing: 2px; color: #C5A55A; font-weight: 700; margin-bottom: 6px; }
  .blog-fullhtml .insight p { font-size: 0.95em; line-height: 1.55; margin: 0; color: rgba(255,255,255,.92); }
  .blog-fullhtml .insight strong { color: #C5A55A; }

  .blog-fullhtml .vis-container { margin: 28px -40px; padding: 22px 24px 18px; background: #fff; border: 1px solid #E8E4ED; border-radius: 14px; box-shadow: 0 4px 18px rgba(45,32,68,0.06); font-family: 'Source Sans 3', -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  @media (max-width: 800px) { .blog-fullhtml .vis-container { margin: 24px 0; } }
  .blog-fullhtml .vis-title { font-family: 'Playfair Display', Georgia, serif; font-size: 1.2rem; color: #2D2044; font-weight: 700; margin: 0; }
  .blog-fullhtml .vis-title::after { content:''; display:block; width:60px; height:3px; background:#C5A55A; margin-top:7px; border-radius:2px; }
  .blog-fullhtml .vis-subtitle { color: #888; font-size: 0.92em; margin-top: 6px; margin-bottom: 14px; font-style: italic; }

  .blog-fullhtml .compare-table { width: 100%; border-collapse: collapse; font-size: 0.88em; margin: 12px 0; font-family: -apple-system, 'Helvetica Neue', Arial, sans-serif; }
  .blog-fullhtml .compare-table th { background: #F8F6FA; padding: 8px 10px; text-align: left; color: #2D2044; font-weight: 700; border-bottom: 2px solid #C5A55A; }
  .blog-fullhtml .compare-table td { padding: 7px 10px; vertical-align: top; border-bottom: 1px solid #eee; color: #333; }
  .blog-fullhtml .compare-table .yes { color: #1E8449; font-weight: 700; }
  .blog-fullhtml .compare-table .no { color: #C0392B; font-weight: 700; }
  .blog-fullhtml .compare-table .partial { color: #E67E22; font-weight: 700; }

  .blog-fullhtml .blog-container { max-width: 760px; margin: 40px auto; padding: 0 20px; }
  .blog-fullhtml .blog-footer { text-align: center; padding: 32px 20px; font-family: 'Helvetica Neue', Arial, sans-serif; font-size: 0.82rem; color: #888; border-top: 1px solid #eee; }

  html[data-theme="dark"] .blog-fullhtml { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml h1 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h2 { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml h3 { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml strong { color: #e0e8f0; }
  html[data-theme="dark"] .blog-fullhtml em { color: #d0d8e8; }
  html[data-theme="dark"] .blog-fullhtml a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.3); }
  html[data-theme="dark"] .blog-fullhtml a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .subtitle { color: #b0b8c8; }
  html[data-theme="dark"] .blog-fullhtml hr { border-top-color: #2a3a3a; }
  html[data-theme="dark"] .blog-fullhtml code { background: #2c3237; color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml blockquote { background: #142a2a; border-left-color: #4dd0ce; color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .paper-tag { background: #2a2410; color: #d4b870; }
  html[data-theme="dark"] .blog-fullhtml .series-nav { background: #0f2625; border-left-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-nav strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-nav .series-nav-links { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-nav a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.3); }
  html[data-theme="dark"] .blog-fullhtml .series-nav a:hover { color: #80e0de; }
  html[data-theme="dark"] .blog-fullhtml .series-footer { background: #142a2a; border-color: #1e4a48; }
  html[data-theme="dark"] .blog-fullhtml .series-footer strong { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .series-footer p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .series-footer a { color: #4dd0ce; border-bottom-color: rgba(77,208,206,0.3); }
  html[data-theme="dark"] .blog-fullhtml .family-callout.heuristic { background: #2a2010; border-left-color: #d4740e; }
  html[data-theme="dark"] .blog-fullhtml .family-callout.value { background: #141e2e; border-left-color: #4682b4; }
  html[data-theme="dark"] .blog-fullhtml .family-callout.ranking { background: #0f2625; border-left-color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .family-callout.heuristic .label { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .family-callout.value .label { color: #6aafe6; }
  html[data-theme="dark"] .blog-fullhtml .family-callout.ranking .label { color: #4dd0ce; }
  html[data-theme="dark"] .blog-fullhtml .family-callout h3 { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .family-callout p { color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .insight { background: #1e1a30; color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml .insight .label { color: #d4b870; }
  html[data-theme="dark"] .blog-fullhtml .insight strong { color: #d4b870; }
  html[data-theme="dark"] .blog-fullhtml .insight p { color: #c9c9ca; }
  html[data-theme="dark"] .blog-fullhtml .vis-container { background: #1e1a30; border-color: #2a2540; }
  html[data-theme="dark"] .blog-fullhtml .vis-title { color: #c5aae8; }
  html[data-theme="dark"] .blog-fullhtml .vis-title::after { background: #d4b870; }
  html[data-theme="dark"] .blog-fullhtml .vis-subtitle { color: #9888a8; }
  html[data-theme="dark"] .blog-fullhtml .compare-table th { background: #15101e; color: #c5aae8; border-bottom-color: #d4b870; }
  html[data-theme="dark"] .blog-fullhtml .compare-table td { border-bottom-color: #2a2540; color: #a8b8b8; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .yes { color: #5cbf5c; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .no { color: #e06060; }
  html[data-theme="dark"] .blog-fullhtml .compare-table .partial { color: #e8a040; }
  html[data-theme="dark"] .blog-fullhtml .blog-footer { color: #9888a8; border-top-color: #2a2540; }
---

<article class="blog-container">

<div class="series-nav">
    <strong>Learning for Planning &middot; Part 2 of 4 &mdash; survey: what to learn</strong>
    <div class="series-nav-links">
        &larr; <a href="/blog/2026/learning-for-planning-scaling-problem/">Part 1: The Scaling Problem + Two Axes</a> &middot; Next: <a href="/blog/2026/learning-for-planning-state-as-graph/">Part 3: Graph Representations Survey &rarr;</a>
    </div>
</div>

<h1>What to Learn — A Survey of Learning Objectives</h1>
<p class="subtitle">Three families of methods, each picking a different target for the model to predict. The choice of target ends up mattering more than the choice of architecture.</p>

<hr>

<p>Part 1 ended with two design axes that every neural method in Learning for Planning has to settle: <strong>what to learn</strong> (the prediction target) and <strong>how to represent state</strong> (the input encoding). This post takes the first axis seriously and walks through the literature. Part 3 will do the same for the second axis.</p>

<p>Three families exist. Each makes a different bet about what a neural network can usefully predict for a planning problem, and each inherits a different set of pathologies. None of them is obviously right; all of them have been pursued for over a decade. Listing them up front:</p>

<div class="family-callout heuristic">
    <div class="label">Family 1</div>
    <h3>Learn a heuristic h(s) for search guidance</h3>
    <p>The model predicts an estimate of the cost-to-goal for a state. A classical search algorithm (A*, GBFS) consumes the prediction and does the actual planning. The learner accelerates search; it does not replace it.</p>
</div>

<div class="family-callout value">
    <div class="label">Family 2</div>
    <h3>Learn a value function V(s); act greedily</h3>
    <p>The model predicts cost-to-goal accurately enough to be used directly. At test time the policy enumerates successor states, looks up V on each, and picks the one with the lowest value. No search tree, no backtracking.</p>
</div>

<div class="family-callout ranking">
    <div class="label">Family 3</div>
    <h3>Learn a ranking — over states, or over actions</h3>
    <p>The model predicts only <em>relative orderings</em> rather than absolute distances. Two variants: rank states (used inside GBFS), or rank actions directly given the current state (used as a reactive policy). The action-ranking variant is where GABAR (Post 4) lives.</p>
</div>

<p>The three families form a progression on a single dimension: <em>how much absolute information the model is asked to predict</em>. Heuristics need to be roughly right and the search engine fixes the rest. Value functions need to be globally accurate. Rankings need only be locally correct. The further down the list you go, the easier the learning target — and the more you give up in correctness guarantees that the search engine used to provide.</p>

<p>The rest of this post walks each family with concrete papers and what they showed. By the end you should have a clear sense of what each family <em>can</em> do, and the specific failure modes that drove the field toward the next family.</p>

<hr>

<h2>Family 1: Learning Heuristics for Search</h2>

<p>The earliest neural approaches to L4P inherit directly from classical heuristic search. The architecture is unchanged: A* or greedy best-first search expands states from a priority queue, ordered by a heuristic estimate of cost-to-goal. The only difference is where the heuristic comes from. Instead of being hand-designed (as h<sub>FF</sub> and h<sub>add</sub> were for decades), it is learned from planner-solved instances.</p>

<p>The bet is conservative: keep all the guarantees of classical search, just replace one component. If the learned heuristic is informative, search expands fewer nodes and runs faster; if it's not, the search algorithm degrades gracefully toward exhaustive enumeration but still finds a solution. The learner is a speedup, not a replacement.</p>

<h3>ASNets — alternating action/proposition layers</h3>

<p><span class="paper-tag">Toyer, Thiébaux, Trevizan, Xie 2020</span> introduced <strong>Action Schema Networks</strong>, the first neural architecture explicitly designed for generalized planning. The key insight: a planning domain has a fixed set of action <em>schemas</em> (e.g., <code>move</code>, <code>pick</code>, <code>drop</code>) regardless of how many objects exist in any particular instance. If the network has one weight set per schema and shares those weights across all groundings of that schema, the same network can process problems of any size.</p>

<p>ASNets stack alternating layers: action layers (one unit per ground action, weighted by which propositions are its preconditions) and proposition layers (one unit per ground proposition, weighted by which actions affect it). After several alternations, each ground-action unit has aggregated information from a fixed-depth neighborhood of the action-proposition graph. The output is interpretable as either a heuristic or an action policy, depending on training objective.</p>

<p>The result was the first demonstration that a neural architecture could generalize across planning instance sizes — small training instances, larger test instances — without retraining. It established the now-standard evaluation protocol the field has used since.</p>

<p>The limitation that subsequent work has chipped at: the fixed alternation depth bounds the network's receptive field. Long-horizon dependencies — where a useful action's effect ripples through many propositions — exceed what a small stack can model. ASNets does well on domains with local effects (Gripper, Blocks small) but struggles where chains of reasoning matter.</p>

<div class="vis-container">
    <h3 class="vis-title">Family 1 on the warehouse &mdash; learned h() guides A*</h3>
    <div class="vis-subtitle">Same 4-zone warehouse from Part 1. ASNets-style heuristic predicts cost-to-goal for each frontier state; A* expands the lowest-h state next. The search tree still exists; the network just orders it.</div>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#8b6914; margin-bottom:6px;">Warehouse state</div>
            <svg viewBox="0 0 260 175" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs><pattern id="th1" width="13" height="13" patternUnits="userSpaceOnUse"><rect width="13" height="13" fill="#F0EDF3"/></pattern></defs>
                <rect x="10" y="4" width="240" height="86" rx="5" fill="none" stroke="#D4CDE0" stroke-width=".8"/>
                <rect x="12" y="6" width="116" height="40" rx="3" fill="url(#th1)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="32" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">Zone A</text>
                <rect x="132" y="6" width="116" height="40" rx="3" fill="url(#th1)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="32" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">Zone B</text>
                <rect x="12" y="50" width="116" height="40" rx="3" fill="url(#th1)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="76" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">Zone C</text>
                <rect x="132" y="50" width="116" height="40" rx="3" fill="url(#th1)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="76" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">Zone D</text>
                <g transform="translate(70,22)"><rect x="-5" y="-7" width="10" height="6" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-6" y="-2" width="12" height="9" rx="1.4" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>
                <rect x="62" y="58" width="16" height="12" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".6"/><text x="70" y="67" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                <rect x="182" y="58" width="16" height="12" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".55"/><text x="190" y="66" text-anchor="middle" fill="#1E8449" font-size="5" font-weight="700" opacity=".7">GOAL</text>
                <text x="130" y="115" text-anchor="middle" fill="#888" font-size="9" font-style="italic">Trained on this 4-zone instance &middot;</text>
                <text x="130" y="128" text-anchor="middle" fill="#888" font-size="9" font-style="italic">tested on 16-zone variant</text>
                <text x="130" y="155" text-anchor="middle" fill="#d4a017" font-size="10" font-weight="700">h(s) = predicted cost-to-goal</text>
                <text x="130" y="168" text-anchor="middle" fill="#8b6914" font-size="8.5" font-style="italic">(learned from solved instances)</text>
            </svg>
        </div>
        <div style="flex:1.1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#8b6914; margin-bottom:6px;">A* search tree (guided by learned h)</div>
            <svg viewBox="0 0 320 230" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <!-- Tree edges -->
                <line x1="160" y1="28" x2="60" y2="78" stroke="#d4a017" stroke-width="2.5" opacity=".55"/>
                <line x1="160" y1="28" x2="160" y2="78" stroke="#D4CDE0" stroke-width="1.2"/>
                <line x1="160" y1="28" x2="260" y2="78" stroke="#D4CDE0" stroke-width="1.2"/>
                <line x1="60" y1="92" x2="30" y2="132" stroke="#D4CDE0" stroke-width="1"/>
                <line x1="60" y1="92" x2="90" y2="132" stroke="#d4a017" stroke-width="2.5" opacity=".55"/>
                <line x1="90" y1="146" x2="90" y2="190" stroke="#d4a017" stroke-width="2.5" opacity=".55"/>

                <!-- Root -->
                <circle cx="160" cy="24" r="20" fill="#fff" stroke="#2D2044" stroke-width="1.5"/>
                <text x="160" y="22" text-anchor="middle" fill="#2D2044" font-size="11" font-weight="700">s₀</text>
                <text x="160" y="33" text-anchor="middle" fill="#d4a017" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">h=3</text>
                <text x="160" y="11" text-anchor="middle" fill="#888" font-size="7" font-style="italic">Bot:A, Pkg:C</text>

                <!-- Level 1 -->
                <circle cx="60" cy="86" r="18" fill="#fef5e7" stroke="#d4a017" stroke-width="1.7"/>
                <text x="60" y="84" text-anchor="middle" fill="#2D2044" font-size="10" font-weight="700">s₁</text>
                <text x="60" y="95" text-anchor="middle" fill="#d4a017" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">h=2</text>
                <text x="60" y="65" text-anchor="middle" fill="#888" font-size="7" font-style="italic">move(A,C)</text>

                <circle cx="160" cy="86" r="16" fill="#fff" stroke="#D4CDE0" stroke-width="1.2"/>
                <text x="160" y="84" text-anchor="middle" fill="#888" font-size="10" font-weight="600">s₂</text>
                <text x="160" y="95" text-anchor="middle" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">h=4</text>

                <circle cx="260" cy="86" r="16" fill="#fff" stroke="#D4CDE0" stroke-width="1.2"/>
                <text x="260" y="84" text-anchor="middle" fill="#888" font-size="10" font-weight="600">s₃</text>
                <text x="260" y="95" text-anchor="middle" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">h=4</text>

                <!-- Level 2 -->
                <circle cx="30" cy="140" r="14" fill="#fff" stroke="#D4CDE0" stroke-width="1" opacity=".5"/>
                <text x="30" y="138" text-anchor="middle" fill="#888" font-size="9">s₄</text>
                <text x="30" y="148" text-anchor="middle" fill="#888" font-size="8" font-family="JetBrains Mono,monospace">h=3</text>

                <circle cx="90" cy="140" r="16" fill="#fef5e7" stroke="#d4a017" stroke-width="1.5"/>
                <text x="90" y="138" text-anchor="middle" fill="#2D2044" font-size="9" font-weight="700">s₅</text>
                <text x="90" y="148" text-anchor="middle" fill="#d4a017" font-size="8.5" font-weight="700" font-family="JetBrains Mono,monospace">h=1</text>
                <text x="123" y="135" text-anchor="middle" fill="#888" font-size="7" font-style="italic">pickup</text>

                <!-- Goal -->
                <circle cx="90" cy="200" r="16" fill="#E3F5EC" stroke="#1E8449" stroke-width="1.7"/>
                <text x="90" y="204" text-anchor="middle" fill="#1E8449" font-size="9" font-weight="700">GOAL</text>
                <text x="135" y="178" text-anchor="middle" fill="#888" font-size="7" font-style="italic">move(C,D)</text>

                <!-- Legend bar -->
                <rect x="195" y="195" width="115" height="28" rx="4" fill="#fef5e7" stroke="rgba(212,160,23,.4)" stroke-width="1"/>
                <text x="252" y="208" text-anchor="middle" fill="#8b6914" font-size="8" font-weight="700">A* picks lowest-h</text>
                <text x="252" y="218" text-anchor="middle" fill="#8b6914" font-size="7.5" font-style="italic">at every step</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #fef5e7; border-radius: 8px; border-left: 4px solid #d4a017;">
        <p style="font-size: 0.92em; color: #5a4400; line-height: 1.5; margin: 0;"><strong>The search tree still exists.</strong> The learned heuristic just orders which node A* expands next. On a 16-zone, 3-package warehouse the tree is much bigger &mdash; the heuristic stops the explosion from being uniform but doesn't eliminate it. This is the inherent limit of Family 1: the speedup is multiplicative on top of search, not a replacement for it.</p>
    </div>
</div>

<h3>STRIPS-HGN — hypergraph networks for domain-independent heuristics</h3>

<p><span class="paper-tag">Shen, Trevizan, Thiébaux 2020</span> introduced <strong>STRIPS-HGN</strong>, which generalizes the message-passing idea to <em>hypergraphs</em>. A STRIPS action has multiple preconditions and multiple effects — naturally modeled as a hyperedge connecting many propositions, not a pairwise edge. STRIPS-HGN's network operates directly on the hypergraph induced by the planning instance.</p>

<p>The architecture stays domain-independent: the same model trained on one set of domains transfers to held-out domains, no retraining. The output is a heuristic plugged into A*. Compared to ASNets, STRIPS-HGN handles the multi-precondition structure more naturally and shows better cross-domain transfer, at the cost of more expensive per-instance graph construction.</p>

<p>The same fundamental limitation applies: STRIPS-HGN is a heuristic, so search overhead remains. The model accelerates search; it doesn't eliminate the need for it.</p>

<h3>GOOSE — modern grounded and lifted heuristic learning</h3>

<p><span class="paper-tag">Chen, Thiébaux, Trevizan 2024</span> released <strong>GOOSE</strong>, the current strongest GNN-based heuristic learner for classical planning. The contribution is twofold: a careful comparison of grounded vs lifted graph representations (the second-axis question Post 3 will cover), and architectural improvements that make GNN heuristics competitive with hand-engineered ones on benchmark IPC domains.</p>

<p>GOOSE trains on small instances (planner-solved) and integrates as a heuristic with classical search. It is the most thoroughly engineered system in this family and represents the current ceiling for "learn the heuristic, keep classical search" methods.</p>

<div class="insight">
    <div class="label">The pattern in Family 1</div>
    <p>Every Family-1 system inherits the <strong>computational overhead of search at execution time</strong>. The learned heuristic speeds up search; it cannot replace it. For problems where search itself is the bottleneck — when the heuristic is good but the branching factor is enormous — Family 1 hits a wall that better heuristics alone cannot break. That wall is what motivates Family 2.</p>
</div>

<hr>

<h2>Family 2: Learning Value Functions for Greedy Policies</h2>

<p>If a learned heuristic is good enough, why search at all? Just expand the immediate successors of the current state, look up the heuristic on each, and take the action leading to the lowest one. No search tree, no priority queue, no backtracking. The policy is reactive: from any state, one forward pass picks the next action.</p>

<p>This is the Family 2 bet. It promises a much faster planner at test time — no search overhead — at the cost of requiring the learned function to be <em>globally accurate</em>, not just informative. A small ranking error in a heuristic costs a few extra node expansions; the same error in a Family 2 value function picks the wrong action and the policy might never reach the goal.</p>

<h3>GPL — unsupervised generalized policies with GNNs</h3>

<p><span class="paper-tag">Ståhlberg, Bonet, Geffner 2022a</span> introduced <strong>GPL</strong> (Generalized Policy Learning), which trains GNNs to predict V(s) without supervision. The training signal is bootstrapped: the model's own value estimates plus the known transition function produce target values via Bellman-style updates, and the network is trained to be consistent with these. No expert planner is needed during training.</p>

<p>At test time the policy enumerates successor states, runs the GNN on each, and picks the action whose successor has the lowest predicted value. The system avoids search entirely. On small instances within the training distribution, GPL matches expert plan quality.</p>

<p>The cracks appear on larger instances. Value learning's promise rests on global accuracy: the prediction must be roughly right not just for the current state but for every state the policy will visit before reaching the goal. For long horizons, small per-step errors compound — the policy ends up favoring the wrong action when it really matters. Generalization to larger problem sizes degrades sharply because the predicted V scale itself drifts: the absolute cost-to-goal a model learned on 5-zone warehouses doesn't map well to 50-zone warehouses where the actual costs are 10× larger.</p>

<div class="vis-container">
    <h3 class="vis-title">Family 2 on the warehouse &mdash; V() lookup, no search</h3>
    <div class="vis-subtitle">Same warehouse. The network predicts V(s') for each successor state of s₀. Policy picks the action leading to the lowest V. No tree, no priority queue &mdash; one forward pass per successor.</div>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#3d4a9e; margin-bottom:6px;">Current state s₀ &middot; trained scale (3-5 step plans)</div>
            <svg viewBox="0 0 260 220" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs><pattern id="tv1" width="13" height="13" patternUnits="userSpaceOnUse"><rect width="13" height="13" fill="#F0EDF3"/></pattern></defs>
                <rect x="10" y="4" width="240" height="86" rx="5" fill="none" stroke="#D4CDE0" stroke-width=".8"/>
                <rect x="12" y="6" width="116" height="40" rx="3" fill="url(#tv1)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="32" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">A</text>
                <rect x="132" y="6" width="116" height="40" rx="3" fill="url(#tv1)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="32" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">B</text>
                <rect x="12" y="50" width="116" height="40" rx="3" fill="url(#tv1)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="76" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">C</text>
                <rect x="132" y="50" width="116" height="40" rx="3" fill="url(#tv1)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="76" text-anchor="middle" fill="#B0A8C0" font-size="12" font-weight="600">D</text>
                <g transform="translate(70,22)"><rect x="-5" y="-7" width="10" height="6" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-6" y="-2" width="12" height="9" rx="1.4" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>
                <rect x="62" y="58" width="16" height="12" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".6"/><text x="70" y="67" text-anchor="middle" fill="#fff" font-size="6" font-weight="700">PKG</text>
                <rect x="182" y="58" width="16" height="12" rx="2" fill="#E3F5EC" stroke="#1E8449" stroke-width="1" stroke-dasharray="3 2" opacity=".55"/>

                <!-- V() values for current state -->
                <text x="130" y="116" text-anchor="middle" fill="#3d4a9e" font-size="11" font-weight="700">V(s₀) = 3</text>
                <text x="130" y="132" text-anchor="middle" fill="#888" font-size="9" font-style="italic">optimal plan ≈ 3 steps from here</text>

                <!-- Successor enumeration -->
                <line x1="130" y1="142" x2="40" y2="170" stroke="#5b6abf" stroke-width="1.2" opacity=".5"/>
                <line x1="130" y1="142" x2="130" y2="170" stroke="#5b6abf" stroke-width="1.2" opacity=".5"/>
                <line x1="130" y1="142" x2="220" y2="170" stroke="#5b6abf" stroke-width="1.2" opacity=".5"/>

                <rect x="8" y="175" width="64" height="34" rx="4" fill="#ecedfa" stroke="#5b6abf" stroke-width="1.6"/>
                <text x="40" y="187" text-anchor="middle" fill="#2D2044" font-size="8" font-weight="700">move(A,C)</text>
                <text x="40" y="200" text-anchor="middle" fill="#3d4a9e" font-size="10" font-weight="700" font-family="JetBrains Mono,monospace">V'=2 ✓</text>
                <rect x="98" y="175" width="64" height="34" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                <text x="130" y="187" text-anchor="middle" fill="#888" font-size="8">move(A,B)</text>
                <text x="130" y="200" text-anchor="middle" fill="#888" font-size="10" font-family="JetBrains Mono,monospace">V'=4</text>
                <rect x="188" y="175" width="64" height="34" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                <text x="220" y="187" text-anchor="middle" fill="#888" font-size="8">move(A,D)</text>
                <text x="220" y="200" text-anchor="middle" fill="#888" font-size="10" font-family="JetBrains Mono,monospace">V'=4</text>
            </svg>
        </div>
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#C0392B; margin-bottom:6px;">Scaled to 16 zones &middot; same network &middot; same prediction range</div>
            <svg viewBox="0 0 257 220" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs><pattern id="tv2" width="8" height="8" patternUnits="userSpaceOnUse"><rect width="8" height="8" fill="#F0EDF3"/></pattern></defs>
                <rect x="2" y="2" width="253" height="120" rx="4" fill="none" stroke="#D4CDE0" stroke-width=".6"/>
                <!-- 4x4 grid (smaller cells) -->
                <g>
                <rect x="4" y="4" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="67" y="4" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="130" y="4" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="193" y="4" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="4" y="35" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="67" y="35" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="130" y="35" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="193" y="35" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="4" y="66" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="67" y="66" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="130" y="66" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="193" y="66" width="60" height="28" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="4" y="97" width="60" height="22" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="67" y="97" width="60" height="22" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="130" y="97" width="60" height="22" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                <rect x="193" y="97" width="60" height="22" rx="3" fill="url(#tv2)" stroke="#D4CDE0" stroke-width=".4"/>
                </g>
                <!-- Robot, packages, goals scattered -->
                <g transform="translate(34,15)"><rect x="-4.5" y="-6" width="9" height="5.5" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-5.2" y="-1" width="10.4" height="7" rx="1.1" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>
                <rect x="215" y="8" width="14" height="9" rx="1.5" fill="#D68910" stroke="#B7770A" stroke-width=".4"/>
                <rect x="89" y="69" width="14" height="9" rx="1.5" fill="#D68910" stroke="#B7770A" stroke-width=".4"/>
                <rect x="152" y="100" width="14" height="9" rx="1.5" fill="#D68910" stroke="#B7770A" stroke-width=".4"/>

                <!-- Truth vs prediction comparison -->
                <text x="128" y="140" text-anchor="middle" fill="#2D2044" font-size="10" font-weight="700">Truth: optimal plan ≈ 15 steps</text>
                <text x="128" y="155" text-anchor="middle" fill="#C0392B" font-size="11" font-weight="700">GPL predicts V(s) ≈ 4</text>
                <text x="128" y="167" text-anchor="middle" fill="#C0392B" font-size="8.5" font-style="italic">(calibrated to small-instance plan lengths)</text>

                <!-- Wrong choice indication -->
                <rect x="8" y="178" width="244" height="36" rx="4" fill="#FDECEA" stroke="#C0392B" stroke-width="1.4"/>
                <text x="128" y="192" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="700">All successors look V ≈ 4</text>
                <text x="128" y="206" text-anchor="middle" fill="#C0392B" font-size="9" font-weight="600">→ greedy lookahead picks arbitrarily &middot; policy wanders</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #ecedfa; border-radius: 8px; border-left: 4px solid #5b6abf;">
        <p style="font-size: 0.92em; color: #3a4475; line-height: 1.5; margin: 0;"><strong>The greedy lookahead breaks on scale.</strong> Trained on 4-zone instances where V(s) lives in [0, 5], the network simply never produces values in the [10, 20] range that 16-zone optimal plans actually need. Successor states all look indistinguishable. There's no search engine to compensate for the misranking &mdash; the policy commits to whatever action the broken values point at.</p>
    </div>
</div>

<h3>Expressive variants — pushing the architecture</h3>

<p><span class="paper-tag">Ståhlberg, Bonet, Geffner 2022b</span> follow up with <strong>more expressive GNN architectures</strong> for general optimal policy learning, exploring what kinds of policies neural networks of bounded expressivity can represent. The work connects to logical expressiveness results <span class="paper-tag">Barceló et al. 2020</span> that bound GNN representation power to descriptions in C<sub>2</sub> (graded modal logic, a restricted fragment of first-order). For planning problems requiring more expressive reasoning, vanilla message-passing is provably insufficient.</p>

<p><span class="paper-tag">Ståhlberg, Bonet, Geffner 2024</span> extends to <strong>"Beyond C<sub>2</sub>"</strong> architectures that incorporate higher-order features. The empirical gains are real but incremental — and crucially, the expressivity story does not resolve the global-consistency problem of value learning. A more expressive model can fit small training instances better, but value generalization to larger sizes still degrades.</p>

<div class="insight">
    <div class="label">The pattern in Family 2</div>
    <p>Value learning trades search-overhead for <strong>global-accuracy requirements</strong>. The exchange works on instances close to the training distribution. It does not work for size generalization, because the value scale itself changes with problem size: a state two steps from goal in a 4-zone warehouse looks superficially similar to a state two steps from goal in a 40-zone warehouse, but the surrounding combinatorial structure is wildly different, and the value function has to be sensitive to that difference in ways small-instance training cannot teach it.</p>
</div>

<hr>

<h2>Family 3: Learning to Rank</h2>

<p>If global accuracy is the obstacle, weaken the requirement. Don't ask the model to predict absolute distances; just ask it to rank options correctly. This is the Family 3 bet, and it splits into two distinct sub-families: rank <em>states</em> (used inside a search algorithm), or rank <em>actions</em> (used as a reactive policy).</p>

<h3>State ranking inside search</h3>

<p><span class="paper-tag">Garrett, Kaelbling, Lozano-Pérez 2016</span> introduced the idea of <strong>learning to rank states for planning</strong>, using RankSVM over hand-crafted features. Pairs of states (one closer to goal, one further) form the training data; the learned ranker decides which to expand first in GBFS. This relaxes the requirement of learning an accurate distance — only the pairwise ordering needs to be correct.</p>

<p>The idea sat for several years before recent revisitation. <span class="paper-tag">Chrestien, Edelkamp, Komenda, Pevný 2024</span> sharpened it: their paper's title is <em>"Optimize planning heuristics to rank, not to estimate cost-to-goal."</em> They show that for guiding GBFS, the loss function should directly target ranking accuracy rather than cost-to-goal regression. Models trained this way produce strictly better search guidance than the same models trained with mean-squared-error loss against ground-truth costs.</p>

<p><span class="paper-tag">Hao, Trevizan, Thiébaux, Ferber, Hoffmann 2024</span> extends this to <strong>pairwise rankings for GBFS</strong>, in two variants (one is a workshop paper, the other an IJCAI extension). They train networks to predict, given two states, which is closer to the goal. GBFS expands using the predicted pairwise ordering as the priority. The empirical result is consistent: ranking-trained heuristics beat regression-trained heuristics on the same architecture and data, often by large margins on hard instances.</p>

<p>These methods all still use search at test time — they're heuristics, just better-trained ones. They share Family 1's search-overhead limitation. What's new is the realization that <em>ranking is an easier learning target than regression</em>, and that this difference translates into measurable planning performance.</p>

<h3>Action ranking — the policy variant</h3>

<p>The natural next step: skip search entirely. Instead of ranking states to decide which one to expand, rank <em>actions</em> in the current state to decide which one to execute. This collapses the "learn a policy" Family 2 idea into a ranking framing — no absolute values, no global consistency, just "which available action looks best right now."</p>

<p>Several lines of work fall here. <span class="paper-tag">Garg, Bajpai et al. 2019</span> (<strong>size-independent neural transfer for RDDL</strong>) learn to score actions in RDDL planning problems. <span class="paper-tag">Janisch, Pevný, Lisý 2020</span> (<strong>SR-DRL</strong>) use GNNs with autoregressive action decomposition for symbolic relational tasks. <span class="paper-tag">Ståhlberg, Bonet, Geffner 2023</span> (<strong>policy gradients for generalized policies</strong>) train action-selecting networks with policy-gradient methods. <span class="paper-tag">Rivlin, Hazan, Karpas 2020</span> apply deep RL to generalized planning.</p>

<p>What unifies these methods is that they predict the next action directly given the state. They differ in training signal (supervised vs RL), in architecture (GNN vs alternating layers vs custom), and in how much action information they expose to the network.</p>

<h3>GRAPL — the closest precursor to GABAR</h3>

<p><span class="paper-tag">Karia, Srivastava 2021</span> (<strong>GRAPL</strong> — Generalized Relational Action Policy Learning) deserves its own treatment because it is the most direct precursor to GABAR. GRAPL ranks actions using <em>canonical abstractions</em>: objects with identical properties are grouped into equivalence classes, and the network reasons about classes rather than individual objects. The output is a ranking over action <em>parameters</em>, used to construct a complete grounded action.</p>

<p>The critical limitation that distinguishes GABAR from GRAPL: GRAPL selects each action parameter <em>independently</em>. A two-parameter action like <code>transport(package, vehicle)</code> is decomposed into "pick the best package" and "pick the best vehicle" as two separate decisions. The choice of vehicle does not condition on the choice of package.</p>

<p>This breaks on domains where parameters are coupled. In Logistics, the correct vehicle for a transport action depends on which package was selected — they need to be in the same city. GRAPL has no mechanism to express this dependency; the package and vehicle decisions are made in parallel. GABAR's GRU-based decoder (Post 4) fixes this by making parameter selection sequential: the action schema is chosen first, then parameters are picked one at a time, each conditioning on what came before.</p>

<div class="vis-container">
    <h3 class="vis-title">Family 3 on the warehouse &mdash; rank, don't estimate</h3>
    <div class="vis-subtitle">Same warehouse. Two variants of "ranking": rank successor states (still uses search) vs rank actions directly (no search). The action-ranking variant is the GABAR cell.</div>

    <div style="display:flex; gap:14px; align-items:stretch;">
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#00807E; margin-bottom:6px;">State ranking inside GBFS</div>
            <svg viewBox="0 0 260 240" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs><pattern id="tr1" width="13" height="13" patternUnits="userSpaceOnUse"><rect width="13" height="13" fill="#F0EDF3"/></pattern></defs>
                <!-- Mini warehouse -->
                <rect x="10" y="4" width="240" height="70" rx="5" fill="none" stroke="#D4CDE0" stroke-width=".8"/>
                <rect x="12" y="6" width="116" height="32" rx="3" fill="url(#tr1)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="26" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">A</text>
                <rect x="132" y="6" width="116" height="32" rx="3" fill="url(#tr1)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="26" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">B</text>
                <rect x="12" y="42" width="116" height="32" rx="3" fill="url(#tr1)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="62" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">C</text>
                <rect x="132" y="42" width="116" height="32" rx="3" fill="url(#tr1)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="62" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">D</text>
                <g transform="translate(70,18)"><rect x="-5" y="-5" width="10" height="5" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-5.5" y="-0.5" width="11" height="7" rx="1.4" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>
                <rect x="62" y="50" width="16" height="10" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/>

                <!-- Pairwise comparisons -->
                <text x="130" y="92" text-anchor="middle" fill="#00807E" font-size="10" font-weight="700">Network ranks pairs of successor states</text>

                <rect x="14" y="102" width="232" height="26" rx="4" fill="#e0f5f5" stroke="#00A3A1" stroke-width="1.4"/>
                <text x="22" y="118" fill="#2D2044" font-size="9" font-weight="600">s_C ?vs? s_B</text>
                <text x="238" y="118" text-anchor="end" fill="#00807E" font-size="9.5" font-weight="700" font-family="JetBrains Mono,monospace">s_C wins</text>

                <rect x="14" y="132" width="232" height="26" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                <text x="22" y="148" fill="#888" font-size="9">s_C ?vs? s_D</text>
                <text x="238" y="148" text-anchor="end" fill="#00807E" font-size="9.5" font-weight="700" font-family="JetBrains Mono,monospace">s_C wins</text>

                <text x="130" y="174" text-anchor="middle" fill="#888" font-size="9" font-style="italic">→ pop s_C, expand</text>

                <rect x="14" y="186" width="232" height="38" rx="4" fill="#F8F6FA" stroke="#C0A8CC" stroke-width="1"/>
                <text x="130" y="200" text-anchor="middle" fill="#6B5B7B" font-size="8" font-weight="700">Still uses GBFS &mdash; search tree exists</text>
                <text x="130" y="214" text-anchor="middle" fill="#6B5B7B" font-size="8" font-style="italic">but only needs "closer than" judgments</text>
            </svg>
        </div>
        <div style="flex:1; min-width:0;">
            <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#00807E; margin-bottom:6px;">Action ranking (GABAR-style, no search)</div>
            <svg viewBox="0 0 260 240" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                <defs><pattern id="tr2" width="13" height="13" patternUnits="userSpaceOnUse"><rect width="13" height="13" fill="#F0EDF3"/></pattern></defs>
                <rect x="10" y="4" width="240" height="70" rx="5" fill="none" stroke="#D4CDE0" stroke-width=".8"/>
                <rect x="12" y="6" width="116" height="32" rx="3" fill="url(#tr2)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="26" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">A</text>
                <rect x="132" y="6" width="116" height="32" rx="3" fill="url(#tr2)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="26" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">B</text>
                <rect x="12" y="42" width="116" height="32" rx="3" fill="url(#tr2)" stroke="#D4CDE0" stroke-width=".3"/><text x="70" y="62" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">C</text>
                <rect x="132" y="42" width="116" height="32" rx="3" fill="url(#tr2)" stroke="#D4CDE0" stroke-width=".3"/><text x="190" y="62" text-anchor="middle" fill="#B0A8C0" font-size="11" font-weight="600">D</text>
                <g transform="translate(70,18)"><rect x="-5" y="-5" width="10" height="5" rx="1.5" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/><rect x="-5.5" y="-0.5" width="11" height="7" rx="1.4" fill="#7B5E99" stroke="#4A3360" stroke-width=".4"/></g>
                <rect x="62" y="50" width="16" height="10" rx="2" fill="#D68910" stroke="#B7770A" stroke-width=".5"/>

                <text x="130" y="92" text-anchor="middle" fill="#00807E" font-size="10" font-weight="700">Network ranks applicable actions directly</text>

                <rect x="14" y="102" width="232" height="26" rx="4" fill="#E8F4F0" stroke="#1E8449" stroke-width="1.6"/>
                <circle cx="26" cy="115" r="7" fill="#1E8449"/>
                <text x="26" y="118" text-anchor="middle" fill="#fff" font-size="7" font-weight="700">1</text>
                <text x="40" y="119" fill="#1E8449" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">move(A,C)</text>
                <text x="238" y="119" text-anchor="end" fill="#1E8449" font-size="9.5" font-weight="700" font-family="JetBrains Mono,monospace">0.91</text>

                <rect x="14" y="132" width="232" height="22" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                <circle cx="26" cy="143" r="6" fill="#D4CDE0"/>
                <text x="26" y="146" text-anchor="middle" fill="#6B5B7B" font-size="6.5" font-weight="700">2</text>
                <text x="40" y="147" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">move(A,B)</text>
                <text x="238" y="147" text-anchor="end" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">0.34</text>

                <rect x="14" y="158" width="232" height="22" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                <circle cx="26" cy="169" r="6" fill="#D4CDE0"/>
                <text x="26" y="172" text-anchor="middle" fill="#6B5B7B" font-size="6.5" font-weight="700">3</text>
                <text x="40" y="173" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">move(A,D)</text>
                <text x="238" y="173" text-anchor="end" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">0.22</text>

                <rect x="14" y="186" width="232" height="38" rx="4" fill="#F0FFF4" stroke="#1E8449" stroke-width="1.2"/>
                <text x="130" y="200" text-anchor="middle" fill="#1E8449" font-size="8" font-weight="700">No search tree &middot; reactive policy</text>
                <text x="130" y="214" text-anchor="middle" fill="#1E8449" font-size="8" font-style="italic">execute top, observe, re-rank, repeat</text>
            </svg>
        </div>
    </div>

    <div style="margin-top: 12px; padding: 10px 14px; background: #e0f5f5; border-radius: 8px; border-left: 4px solid #00A3A1;">
        <p style="font-size: 0.92em; color: #005a58; line-height: 1.5; margin: 0;"><strong>The action-ranking output stays the same shape regardless of warehouse size.</strong> Same warehouse with 16 zones and 3 packages produces a longer list of candidate actions, but each one still gets a score. The network's prediction target &mdash; "which of these actions is best right here" &mdash; doesn't change scale with the problem. That's the property that lets the small-training-set network rank actions on a 50-zone warehouse without recalibration.</p>
    </div>
</div>

<div class="insight">
    <div class="label">The pattern in Family 3</div>
    <p>Ranking sidesteps the global-consistency problem of Family 2 by predicting only local orderings. The action-ranking sub-family additionally sidesteps Family 1's search overhead by predicting a policy directly. The remaining question is <strong>how to represent the input</strong> so the same ranking model works across problem sizes — which is exactly the Axis-2 question Post 3 covers.</p>
</div>

<hr>

<h2>Cross-family comparison</h2>

<p>Putting the three families side by side along the dimensions that matter most for size generalization:</p>

<div class="vis-container">
    <h3 class="vis-title">The three families at a glance</h3>
    <div class="vis-subtitle">Same planning problem, different prediction target, different inherited limitations.</div>

    <table class="compare-table">
        <thead>
            <tr>
                <th>Property</th>
                <th>Heuristic (Family 1)</th>
                <th>Value Function (Family 2)</th>
                <th>Ranking (Family 3)</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>What the model predicts</td>
                <td>Estimate of cost-to-goal h(s)</td>
                <td>Accurate cost-to-goal V(s)</td>
                <td>Pairwise / total ordering only</td>
            </tr>
            <tr>
                <td>What the search engine does</td>
                <td>Full search, guided by h</td>
                <td>One-step lookahead, no search</td>
                <td>State-rank: still search. Action-rank: no search</td>
            </tr>
            <tr>
                <td>Search overhead at test time</td>
                <td class="no">Yes</td>
                <td class="yes">No</td>
                <td class="partial">Depends on variant</td>
            </tr>
            <tr>
                <td>Requires global prediction accuracy</td>
                <td class="yes">No (graceful degradation)</td>
                <td class="no">Yes</td>
                <td class="yes">No (only local ordering)</td>
            </tr>
            <tr>
                <td>Degrades gracefully on size scale-up</td>
                <td class="yes">Yes (slower search, still correct)</td>
                <td class="no">No (wrong action choices)</td>
                <td class="partial">Action variants: yes; state variants: partial</td>
            </tr>
            <tr>
                <td>Representative papers</td>
                <td>ASNets, STRIPS-HGN, GOOSE</td>
                <td>GPL, Expressive-v1/v2</td>
                <td>RankSVM, Chrestien et al., GBFS-rank, GRAPL, GABAR</td>
            </tr>
        </tbody>
    </table>
</div>

<p>The progression across families is a progressive relaxation of what the learner is responsible for. Family 1 hands almost everything to the search engine and only learns guidance. Family 2 takes everything: search disappears, but global accuracy becomes mandatory. Family 3 hits a sweet spot — no search overhead, no global accuracy requirement, just "rank what's in front of you."</p>

<p>The cost is that Family 3 gives up the search engine's correctness guarantee. A misranked action might lead the policy into a dead end, and there's no explicit mechanism to back out. In practice, this is handled by simple termination conditions (don't revisit states; cap execution length), but it remains a real difference from Family 1's behavior.</p>

<h2>Back to the warehouse: three families, one state</h2>

<p>To make the three families concrete, here is the same warehouse state &mdash; robot in A, package on C, goal to deliver to D &mdash; passed to a representative of each family. They produce different outputs and use them differently.</p>

<div class="vis-container">
    <h3 class="vis-title">Same warehouse state, three predictions</h3>
    <div class="vis-subtitle">Family 1 predicts h(s) for nodes in a search tree. Family 2 predicts V(s) for each successor. Family 3 ranks actions in the current state.</div>

    <div style="display:flex; gap:14px; align-items:stretch;" class="three-fam-row">
        <!-- Family 1 -->
        <div style="flex:1; border:1.5px solid rgba(212,160,23,.35); border-radius:10px; overflow:hidden; display:flex; flex-direction:column;">
            <div style="background:#fef5e7; padding:10px 14px; border-bottom:1px solid rgba(212,160,23,.3);">
                <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#8b6914;">Family 1</div>
                <div style="font-family:'Playfair Display',Georgia,serif; font-size:1rem; color:#2D2044; font-weight:700;">Heuristic h(s)</div>
                <div style="font-size:.78rem; color:#7a5e1f; margin-top:2px; font-style:italic;">Predict cost-to-goal; A* uses it to order the queue</div>
            </div>
            <div style="padding:10px; flex:1;">
                <svg viewBox="0 0 200 240" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                    <!-- Mini warehouse at top -->
                    <rect x="10" y="6" width="80" height="36" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".5"/>
                    <text x="22" y="22" fill="#B0A8C0" font-size="8" font-weight="600">A</text>
                    <text x="60" y="22" fill="#B0A8C0" font-size="8" font-weight="600">B</text>
                    <text x="22" y="38" fill="#B0A8C0" font-size="8" font-weight="600">C</text>
                    <text x="60" y="38" fill="#B0A8C0" font-size="8" font-weight="600">D</text>
                    <rect x="100" y="6" width="90" height="36" rx="3" fill="#fef5e7" stroke="rgba(212,160,23,.4)" stroke-width=".8"/>
                    <text x="145" y="20" text-anchor="middle" fill="#7a5e1f" font-size="7" font-weight="700">Search tree</text>
                    <text x="145" y="32" text-anchor="middle" fill="#7a5e1f" font-size="6.5" font-style="italic">guided by h</text>

                    <!-- Search tree -->
                    <line x1="100" y1="74" x2="50" y2="110" stroke="#d4a017" stroke-width="2" opacity=".7"/>
                    <line x1="100" y1="74" x2="100" y2="110" stroke="#D4CDE0" stroke-width="1"/>
                    <line x1="100" y1="74" x2="150" y2="110" stroke="#D4CDE0" stroke-width="1"/>
                    <line x1="50" y1="124" x2="50" y2="160" stroke="#d4a017" stroke-width="2" opacity=".7"/>
                    <line x1="50" y1="174" x2="50" y2="210" stroke="#d4a017" stroke-width="2" opacity=".7"/>
                    <circle cx="100" cy="70" r="14" fill="#fff" stroke="#2D2044" stroke-width="1.2"/>
                    <text x="100" y="68" text-anchor="middle" fill="#2D2044" font-size="8" font-weight="700">s₀</text>
                    <text x="100" y="78" text-anchor="middle" fill="#d4a017" font-size="7" font-weight="700">h=3</text>
                    <circle cx="50" cy="120" r="12" fill="#fef5e7" stroke="#d4a017" stroke-width="1.4"/>
                    <text x="50" y="118" text-anchor="middle" fill="#2D2044" font-size="7" font-weight="700">s₁</text>
                    <text x="50" y="127" text-anchor="middle" fill="#d4a017" font-size="6.5" font-weight="700">h=2</text>
                    <circle cx="100" cy="120" r="11" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                    <text x="100" y="119" text-anchor="middle" fill="#888" font-size="7">s₂</text>
                    <text x="100" y="127" text-anchor="middle" fill="#888" font-size="6.5">h=4</text>
                    <circle cx="150" cy="120" r="11" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                    <text x="150" y="119" text-anchor="middle" fill="#888" font-size="7">s₃</text>
                    <text x="150" y="127" text-anchor="middle" fill="#888" font-size="6.5">h=4</text>
                    <circle cx="50" cy="170" r="11" fill="#fef5e7" stroke="#d4a017" stroke-width="1.3"/>
                    <text x="50" y="169" text-anchor="middle" fill="#2D2044" font-size="7" font-weight="700">s₅</text>
                    <text x="50" y="177" text-anchor="middle" fill="#d4a017" font-size="6.5" font-weight="700">h=1</text>
                    <circle cx="50" cy="216" r="11" fill="#E3F5EC" stroke="#1E8449" stroke-width="1.3"/>
                    <text x="50" y="220" text-anchor="middle" fill="#1E8449" font-size="6.5" font-weight="700">GOAL</text>

                    <text x="100" y="237" text-anchor="middle" fill="#7a5e1f" font-size="7" font-style="italic">A* picks lowest h() at every step</text>
                </svg>
            </div>
        </div>

        <!-- Family 2 -->
        <div style="flex:1; border:1.5px solid rgba(91,106,191,.35); border-radius:10px; overflow:hidden; display:flex; flex-direction:column;">
            <div style="background:#ecedfa; padding:10px 14px; border-bottom:1px solid rgba(91,106,191,.3);">
                <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#3d4a9e;">Family 2</div>
                <div style="font-family:'Playfair Display',Georgia,serif; font-size:1rem; color:#2D2044; font-weight:700;">Value V(s)</div>
                <div style="font-size:.78rem; color:#3d4a9e; margin-top:2px; font-style:italic;">Score each successor; pick lowest V</div>
            </div>
            <div style="padding:10px; flex:1;">
                <svg viewBox="0 0 200 240" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                    <rect x="10" y="6" width="80" height="36" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".5"/>
                    <text x="22" y="22" fill="#B0A8C0" font-size="8" font-weight="600">A</text>
                    <text x="60" y="22" fill="#B0A8C0" font-size="8" font-weight="600">B</text>
                    <text x="22" y="38" fill="#B0A8C0" font-size="8" font-weight="600">C</text>
                    <text x="60" y="38" fill="#B0A8C0" font-size="8" font-weight="600">D</text>
                    <rect x="100" y="6" width="90" height="36" rx="3" fill="#ecedfa" stroke="rgba(91,106,191,.4)" stroke-width=".8"/>
                    <text x="145" y="20" text-anchor="middle" fill="#3d4a9e" font-size="7" font-weight="700">One-step lookahead</text>
                    <text x="145" y="32" text-anchor="middle" fill="#3d4a9e" font-size="6.5" font-style="italic">enumerate successors</text>

                    <text x="100" y="62" text-anchor="middle" fill="#2D2044" font-size="8" font-weight="700">Current state s₀</text>
                    <line x1="100" y1="68" x2="100" y2="82" stroke="#D4CDE0" stroke-width="1"/>
                    <text x="100" y="92" text-anchor="middle" fill="#888" font-size="7" font-style="italic">Successor states:</text>

                    <!-- Three successor rows -->
                    <rect x="20" y="103" width="160" height="22" rx="4" fill="#ecedfa" stroke="#5b6abf" stroke-width="1.4"/>
                    <text x="28" y="118" fill="#2D2044" font-size="8" font-weight="600">→ Bot:C, Pkg:C</text>
                    <text x="172" y="118" text-anchor="end" fill="#3d4a9e" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">V=2 ✓</text>

                    <rect x="20" y="132" width="160" height="22" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                    <text x="28" y="147" fill="#888" font-size="8">→ Bot:B, Pkg:C</text>
                    <text x="172" y="147" text-anchor="end" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">V=4</text>

                    <rect x="20" y="161" width="160" height="22" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                    <text x="28" y="176" fill="#888" font-size="8">→ Bot:D, Pkg:C</text>
                    <text x="172" y="176" text-anchor="end" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">V=4</text>

                    <rect x="20" y="195" width="160" height="24" rx="4" fill="#F0FFF4" stroke="#1E8449" stroke-width="1.2"/>
                    <text x="100" y="210" text-anchor="middle" fill="#1E8449" font-size="8" font-weight="700">→ take move(A,C)</text>
                    <text x="100" y="237" text-anchor="middle" fill="#3d4a9e" font-size="7" font-style="italic">No search tree, just lookups</text>
                </svg>
            </div>
        </div>

        <!-- Family 3 -->
        <div style="flex:1; border:1.5px solid rgba(0,163,161,.4); border-radius:10px; overflow:hidden; display:flex; flex-direction:column;">
            <div style="background:#e0f5f5; padding:10px 14px; border-bottom:1px solid rgba(0,163,161,.3);">
                <div style="font-size:.62rem; text-transform:uppercase; letter-spacing:1.5px; font-weight:700; color:#00807E;">Family 3</div>
                <div style="font-family:'Playfair Display',Georgia,serif; font-size:1rem; color:#2D2044; font-weight:700;">Action ranking</div>
                <div style="font-size:.78rem; color:#00807E; margin-top:2px; font-style:italic;">Score each action; pick top</div>
            </div>
            <div style="padding:10px; flex:1;">
                <svg viewBox="0 0 200 240" preserveAspectRatio="xMidYMid meet" style="width:100%;height:auto;">
                    <rect x="10" y="6" width="80" height="36" rx="3" fill="#F0EDF3" stroke="#D4CDE0" stroke-width=".5"/>
                    <text x="22" y="22" fill="#B0A8C0" font-size="8" font-weight="600">A</text>
                    <text x="60" y="22" fill="#B0A8C0" font-size="8" font-weight="600">B</text>
                    <text x="22" y="38" fill="#B0A8C0" font-size="8" font-weight="600">C</text>
                    <text x="60" y="38" fill="#B0A8C0" font-size="8" font-weight="600">D</text>
                    <rect x="100" y="6" width="90" height="36" rx="3" fill="#e0f5f5" stroke="rgba(0,163,161,.4)" stroke-width=".8"/>
                    <text x="145" y="20" text-anchor="middle" fill="#00807E" font-size="7" font-weight="700">Direct action scoring</text>
                    <text x="145" y="32" text-anchor="middle" fill="#00807E" font-size="6.5" font-style="italic">no successor enumeration</text>

                    <text x="100" y="62" text-anchor="middle" fill="#2D2044" font-size="8" font-weight="700">Current state s₀</text>
                    <text x="100" y="92" text-anchor="middle" fill="#888" font-size="7" font-style="italic">Applicable actions, ranked:</text>

                    <!-- Three ranked actions -->
                    <rect x="20" y="103" width="160" height="22" rx="4" fill="#E8F4F0" stroke="#1E8449" stroke-width="1.4"/>
                    <circle cx="32" cy="114" r="7" fill="#1E8449"/>
                    <text x="32" y="117" text-anchor="middle" fill="#fff" font-size="7" font-weight="700">1</text>
                    <text x="45" y="118" fill="#1E8449" font-size="8" font-weight="700" font-family="JetBrains Mono,monospace">move(A,C)</text>
                    <text x="172" y="118" text-anchor="end" fill="#1E8449" font-size="9" font-weight="700" font-family="JetBrains Mono,monospace">0.91</text>

                    <rect x="20" y="132" width="160" height="22" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                    <circle cx="32" cy="143" r="7" fill="#D4CDE0"/>
                    <text x="32" y="146" text-anchor="middle" fill="#6B5B7B" font-size="7" font-weight="700">2</text>
                    <text x="45" y="147" fill="#888" font-size="8" font-family="JetBrains Mono,monospace">move(A,B)</text>
                    <text x="172" y="147" text-anchor="end" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">0.34</text>

                    <rect x="20" y="161" width="160" height="22" rx="4" fill="#fff" stroke="#D4CDE0" stroke-width="1"/>
                    <circle cx="32" cy="172" r="7" fill="#D4CDE0"/>
                    <text x="32" y="175" text-anchor="middle" fill="#6B5B7B" font-size="7" font-weight="700">3</text>
                    <text x="45" y="176" fill="#888" font-size="8" font-family="JetBrains Mono,monospace">move(A,D)</text>
                    <text x="172" y="176" text-anchor="end" fill="#888" font-size="9" font-family="JetBrains Mono,monospace">0.22</text>

                    <rect x="20" y="195" width="160" height="24" rx="4" fill="#F0FFF4" stroke="#1E8449" stroke-width="1.2"/>
                    <text x="100" y="210" text-anchor="middle" fill="#1E8449" font-size="8" font-weight="700">→ take move(A,C)</text>
                    <text x="100" y="237" text-anchor="middle" fill="#00807E" font-size="7" font-style="italic">Reactive: state → ranked actions</text>
                </svg>
            </div>
        </div>
    </div>

    <div style="margin-top: 14px; padding: 10px 14px; background: #F8F6FA; border-radius: 8px; border-left: 4px solid #C5A55A;">
        <p style="font-size: 0.92em; color: #2D2044; line-height: 1.5; margin: 0;">All three pick the same action <code>move(A, C)</code> on this small instance &mdash; the warehouse is too easy for the families to disagree. The disagreements start at scale: Family 1 still works but the search tree blows up, Family 2's value scale drifts and the lookahead misranks, Family 3 keeps producing valid rankings because <em>only the local ordering matters</em>, and that doesn't change scale with the problem.</p>
    </div>
</div>

<h2>What this means for size generalization</h2>

<p>The original motivation for L4P is to train on small instances (where planners are fast) and deploy on large ones (where they are not). Re-reading the three families with size generalization as the criterion:</p>

<ul>
    <li><strong>Family 1</strong> generalizes acceptably because the search engine compensates for heuristic errors. Even on large instances, a slightly off heuristic still produces correct plans — just slower. The bottleneck remains search.</li>
    <li><strong>Family 2</strong> generalizes poorly. The value-scale shift between small and large instances breaks the prediction, and there's no recovery mechanism: a wrong action choice is wrong, and the next state inherits the error.</li>
    <li><strong>Family 3 (action ranking)</strong> generalizes well because the prediction target — "rank the actions available right here, right now" — does not change scale with the problem. A 4-zone warehouse and a 400-zone warehouse have the same kinds of actions to rank; only the candidate set is larger.</li>
</ul>

<p>This is the empirical story the field has been converging on for several years. Family 1 hits search-overhead walls. Family 2 hits global-consistency walls. Family 3, when paired with the right input representation, doesn't hit either.</p>

<h2>The other axis</h2>

<p>Every paper in this post made a choice on Axis 2 as well: how to represent the input state. Most of them use GNNs over relational structures, but the specific graph construction varies widely. ASNets uses alternating action-proposition layers. STRIPS-HGN uses hypergraphs. GPL uses object-only graphs without explicit action nodes. GRAPL uses canonically abstracted graphs. GOOSE compares grounded vs lifted constructions head-to-head.</p>

<p>These choices are <em>orthogonal</em> to the learning-objective choice. The same Family-3 ranking objective can sit on top of many different graph encodings, and the encoding choice determines whether the same architecture can read instances of different sizes. Post 3 takes this seriously: it surveys the representation axis with the same care this post gave the objective axis.</p>

<h2>Why GABAR sits where it does</h2>

<p>One reason to read this survey before reading the GABAR paper deep-dive in Post 4: GABAR's place in the literature only makes sense once the three families and their limitations are concrete. GABAR is a Family-3 action-ranking method. It uses an action-centric graph representation (the Axis-2 choice covered in Post 3). It adds a GRU-based decoder for sequential parameter selection (the GRAPL limitation fix). And it explicitly trains the network to produce action rankings consistent with planner-solved demonstrations.</p>

<p>Each of those choices is a response to a specific limitation in prior work. GABAR is not "yet another GNN for planning" — it's a recipe that picks the right cell in the two-axis design space and adds the one missing piece (sequential decoding) that prior action-ranking methods lacked.</p>

<hr>

<div class="series-footer">
    <strong>Where this fits</strong>
    <p>This is the first of the two survey posts. Part 3 surveys the representation axis (Axis 2): how to encode a planning state as a graph the network can read. The two surveys together set up everything you need to read the GABAR paper deep-dive (Part 4) and understand why each of GABAR's design choices was the right one.</p>
    <p style="margin-top: 10px; font-size: 0.85em; color: #666;">&larr; <a href="/blog/2026/learning-for-planning-scaling-problem/">Part 1: The Scaling Problem + Two Axes</a> &middot; <a href="/blog/2026/learning-for-planning-state-as-graph/">Part 3: Graph Representations Survey &rarr;</a></p>
</div>

</article>
